##### socket
任何网络通信基于socket实现，socket在传输层和应用层之间。

传输层协议常用的有两种：tcp和udp
tcp一开始连接有三次握手，之后才通信，通信完了在进行四次断开，是一种非常可靠的协议
udp是一种无连接协议，不包装数据，不确认是否接受，所以udp没有可靠性保证，顺序的保证。


借助python来了解socket：

client:
创建客户端对象->发送连接请求
```
import socket
sk=socket.socket()
sk.connect(("127.0.0.1",9999))
ret=sk.recv(1024)
print(ret)
sk.send(b"ok")
sk.close()
```

server:
创建服务端socket对象->绑定地址->监听端口->等待接收请求
```
import socket

sk=socket.socket()
sk.bind(("127.0.0.1",9999))
sk.listen(5)      #
conn,addr=sk.accept()
conn.send(b"hello")
ret=conn.recv(1024)
print(ret)
conn.close()
sk.close()
```

sk.setblocking(flag)  设置不阻塞

##### select的IO多路复用
socket服务端在accept和recv时会卡住，accept等待客户端连接，recv等待消息传入。
这个时候该程序其他事情是做不了的，也不能同时对多个客户端响应，可以利用多线程和多进程解决这个问题，但是线程和进程创建数量有限，想在一个线程中完成同时对多个客户端响应，就可以考虑用IO多路复用。
socket在listen之后开启端口，可以开始被访问，访问后的socket和之前的socket有些变化，这个变化可以被select捕获到，一般情况下accept接收后创建新的socket与客户端连接，服务端的socket被搁置，等到客户端的accept使用close后，或意外断开后才能再次接受其他客户端访问。
accept和recv之前为可读，有变动时被select，传入rlist；recv接收一次后同时也是可写状态，传入wlist。

select传统用法
select.select(rlist,wlist,elist,timeout=-1)->(rlist,wlist,elist)

##### select配合socket实现http服务
浏览器的socket为客户端，服务端开启http服务后，客户端输入地址按回车就是一次socket请求。期间经历：服务端socket用select循环监听，浏览器回车后连接服务端socket，服务端接收到连接后，浏览器立刻发送请求报文（分为请求头部和两行和请求主体）后等待回复，服务端接收后分析报文信息解析相应的html文档，把html文档发给客户端。
```
import gevent
import socket
import select,pprint


class MyHttpServer:
    def __init__(self,url_list):
        self.url_list=url_list
    def request_process(self,sock):
        head_dict={}
        request_str = bytes()
        while True:
            try:
                # 循环接收请求报文，如果接收不到请求，不等待，直接报错，
                # 我捕捉到错误后就退出循环
                chunk = sock.recv(1024)
            except BlockingIOError:
                break
            else:
                request_str += chunk
        request_str = request_str.decode("utf8")
        # http协议头部和主体之间有两行空白
        head, body = request_str.split("\r\n\r\n", 1)
        print(request_str)
        headlist = head.split("\r\n")
        for line in headlist:
            if len(line.split(":"))<2:
                # 除了第一行以外，其他都是以:分割，
                # 第一行的组成部分有<响应方式> <url> <http类型>，
                # 例如：GET /index.html HTTP/1.1
                head_dict["method"],head_dict["url"],head_dict["protocol"]=line.split(" ")
            else:
                k,v=line.split(":",1)
                head_dict[k]=v
        pprint.pprint(head_dict)
        return head_dict,body
    def run(self):
        sock = socket.socket()
        sock.bind(("127.0.0.1", 8888))
        # setblocking指定accept和recv时不阻塞，如果没有连接就直接报错
        sock.setblocking(False)
        sock.listen(5)
        inputs = [sock, ]
        func_name = None
        while True:
            rList, w, e = select.select(inputs, [], [], 0.5)
            for sk in rList:
                # 接收客户端连接
                if sk == sock:
                    conn, addr = sock.accept()
                    inputs.append(conn)
                else:
                    #等待客户端发送报文
                    head_dict,body=self.request_process(sk)
                    request_url=head_dict["url"]
                    #分析url的内容
                    for item in url_list:
                        if request_url==item[0]:
                            print(request_url,item[0])
                            func_name=item[1]
                    if func_name:
                        ret=func_name(head_dict)
                    else:
                        ret="404"
                    # 得到内容后回复
                    sk.sendall(ret.encode("utf8"))
                    del ret
                    func_name=None
                    sk.close()
                    inputs.remove(sk)

        sock.close()
def f1(request):
    return "f1"
def f2(request):
    return "f2"

url_list=[
    ("/index.html",f1),
    ("/login.html",f2),
]
s=MyHttpServer(url_list)
s.run()
```

##### select配合socket实现爬虫
当我要从多个网站爬大量数据时，一个一个爬会浪费很多时间。所以会考虑并发爬，会想到用线程和协程来实现。爬网站的过程：发送连接请求，等待连接通过，发送请求报文，recv等待回复。实质 是一个简易浏览器。
等待连接通过之前可以一次性发送多个socket请求，使用select判断socket请求是否通过，通过后的socket可以写入请求报文，再次通过select判断是否接受到响应报文，select同时监听三个任务。
```
import socket
import select
class pack:
    def __init__(self,socket,host,url,backcall):
        self.socket=socket
        self.host = host
        self.url = url
        self.backcall=backcall
    def fileno(self):
        return self.socket.fileno()


class NbIO:
    def __init__(self):
        self.connections=[]
        self.read_table=[]
        pass
    def connect(self,url_list):
        # 同时创建多个连接，一次性发送
        for item in url_list:
            conn=socket.socket()
            conn.setblocking(False)
            try:
                conn.connect((item["host"], 80))
            except BlockingIOError as e:
                pass
            # select能监听socket，同时也能监听包含socket的对象，select监听实质是fileno文件描述符，
            #
            obj=pack(conn,item["host"],item["url"],item["backcall"])

            self.connections.append(obj)
            self.read_table.append(obj)

    def send(self):
        while True:
            if len(self.read_table)==0:
                break
            rList, wList, eList = select.select(self.read_table, self.connections, [], 0.5)
            # 有数据回来的socket
            for rsock in rList:
                data=bytes()
                while True:
                    try:
                        chunk=rsock.socket.recv(1024)
                    except BlockingIOError as e:
                        break
                    data+=chunk
                # 接收后使用回调函数，拿到的数据进行相应的处理
                rsock.backcall(data)
                self.read_table.remove(rsock)
            # 已经连接到远程的socket
            for wsock in wList:
                # 发送请求报文
                send_msg="GET %s HTTP/1.1\r\nHost: %s\r\n\r\n"%(wsock.url,wsock.host)
                wsock.socket.sendall(send_msg.encode("utf8"))
                # 发送后该socket我可以不用管了，直接删除
                self.connections.remove(wsock)
        print(self.read_table)



def f1(data):
    print(data.decode("gb2312"))

def f2(data):
    print(data.decode("utf8"))

url_list=[
    {"host":"www.51cto.com","url":"/","backcall":f1},
    {"host":"www.baidu.com","url":"/","backcall":f2},
    {"host":"www.doublenactive.cn","url":"/","backcall":f2},
]
s=NbIO()
s.connect(url_list)
s.send()
```

IO多路复用是指通过一种机制，监听多个文件描述符，一旦某个文件描述符等待就绪，就通知程序进行相应读写。IO多路复用机制有select，poll和epoll。
select在linux和windows上通用，可跨平台。select的缺点：能够监视的文件描述符有限制，linux中一般1024。调用select()时会对所有socket扫描一遍，增加开销。
poll和select本质没有多大区别，poll没有最大文件描述符限制，select和poll将就绪的文件描述符告诉进程后，如果进程没有对IO操作，下一次调用时不会丢失就绪消息，称为水平触发
epoll是linux2.6直接支持的机制，只告诉进程哪些文件描述符就绪，只告诉一遍。epoll使用内存映射机制，省掉了文件描述符在系统调用时的复制开销。epoll通过注册一个文件描述符，内核才对文件描述符扫描。