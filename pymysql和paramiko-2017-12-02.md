### pymysql
```
import pymysql
mysql=pymysql.Connection(host="10.0.0.20",port=3306,user="root",password="123456",
                   database="hostmanage",charset="utf8")
cursor=mysql.cursor()

//cursor=mysql.cursor(pymysql.cursors.DictCursor)    #获取的时候是字典

#查找
cursor.execute("select * from users")
print("获取1个",cursor.fetchone())
cursor.scroll(-1)
print("获取2个",cursor.fetchmany(2))
cursor.scroll(0,mode="absolute")
print("获取所有",cursor.fetchall())

#添加
# ret=cursor.execute('''
# insert into users values(
# null,%s,%s,%s
# )
# ''',("雪菜","e10adc3949ba59abbe56e057f20f883e",3))
user_list=[
    ("冬马","e10adc3949ba59abbe56e057f20f883e",2),
    ("小马","e10adc3949ba59abbe56e057f20f883e",1),
    ]
ret=cursor.executemany('''
insert into users values(
null,%s,%s,%s
)
''',user_list)
mysql.commit()

cursor.close()
```

Connection构造函数
```
def __init__(self, host=None, user=None, password="",
                 database=None, port=0, unix_socket=None,
                 charset='', sql_mode=None,
                 read_default_file=None, conv=None, use_unicode=None,
                 client_flag=0, cursorclass=Cursor, init_command=None,
                 connect_timeout=None, ssl=None, read_default_group=None,
                 compress=None, named_pipe=None, no_delay=None,
                 autocommit=False, db=None, passwd=None, local_infile=False,
                 max_allowed_packet=16*1024*1024, defer_connect=False,
                 auth_plugin_map={}, read_timeout=None, write_timeout=None):
```

##### pymysql的Connection类重构
重构的目的是为了按照自身的需求，订制适合自己记忆的方法。也是为了对模块加深印象
如像以上构造函数要这么多参数，一般用到的也就那么几个：
```
class OverrideMysql(Connection):
   #重写构造方法：
    def __init__(self, host, port, username, password, database, charset="utf8"):
        '''**可以直接把dict放入'''
        connections.DEFAULT_CHARSET = charset
        Connection.__init__(self, host=host, port=port, user=username, passwd=password, database=database)
				
    # cursor方法：有两种模式，字典模式需要加入pymysql.cursors.DictCursor参数，我就可以指定传入list或dict，让类内部帮我调用
    def get_cursor(self, type="list"):
        if type == "list":
            self.fast_cursor = self.cursor()
        elif type == "dict":
            self.fast_cursor = self.cursor(pymysql.cursors.DictCursor)

    def execute(self,query,args=()):
        ret_count=self.fast_cursor.execute(query,args)
        return ret_count
    def fetch(self, n=1):
        if n == 1:
            cords = self.fast_cursor.fetchone()
        elif n == -1:
            cords = self.fast_cursor.fetchall()
        elif n > 1:
            cords = self.fast_cursor.fetchmany(n)
        return cords

    def scroll(self, value, mode="relative"):
        '''
        移动指针
        :param value:
        :param mode: relative相对移送，value是移动的数量；
                     absolute绝对移动，value是移动的位置
        :return:
        '''
        self.fast_cursor.scroll(value, mode)
    def commit(self):
        Connection.commit(self)
    def close(self):
        self.fast_cursor.close()
        Connection.close(self)
```

### paramiko
paramiko连接服务器ssh

```
import paramiko
transport = paramiko.Transport(("10.0.0.20",22))
transport.connect(username="euewrqe",password="123456")

ssh=paramiko.SSHClient()
ssh._transport = transport    #绑定transport

stdin,stdout,stderr=ssh.exec_command("ls")
print(stdout.read())
ssh.close()
```

```
def connect(self, hostkey=None, username='', password=None, pkey=None,
                gss_host=None, gss_auth=False, gss_kex=False, gss_deleg_creds=True):
								
```


使用私钥
```
import paramiko
#生成私钥
private_key=paramiko.RSAKey.from_private_key_file("./id_rsa")
#字符串私钥
from io import StringIO
file_obj=StringIO(rsa_string)
private_key=paramiko.RSAKey(file_obj=file_obj)

transport = paramiko.Transport(("10.0.0.20",22))
# 绑定私钥和用户
transport.connect(username="euewrqe",pkey=private_key)

ssh=paramiko.SSHClient()
ssh._transport = transport    #绑定transport

stdin,stdout,stderr=ssh.exec_command("ls")
print(stdout.read())
ssh.close()
```
sftp:
```
import paramiko

private_key=paramiko.RSAKey.from_private_key_file("./id_rsa")

transport = paramiko.Transport(("10.0.0.20",22))
transport.connect(username="euewrqe",pkey=private_key)

sftp=paramiko.SFTPClient.from_transport(transport)
#下载
sftp.get("remotepath","localpath")
#上传
sftp.put("localpath","remotepath")
```

##### paramiko重构

重构的目的：为了整合paramiko的ssh和sftp
```
class FastParamiko:
    #可以选择私钥或密码
    RSAKey = paramiko.rsakey.RSAKey
    def __init__(self,hostname,ip,port,username,password=None,pkey=None):
        '''有密码使用密码，有私钥使用私钥，都定义则密码优先'''
        self.connect_msg={"hostname":hostname,"ip":ip,"port":port,"username":username,"password":password,"pkey":pkey}
        self.ssh=None
        self.output = {}
        self.transport = paramiko.Transport((self.connect_msg["ip"], self.connect_msg["port"]))

        if password:
            self.transport.connect(username=self.connect_msg["username"],
                              password=self.connect_msg["password"])
        elif pkey:
            self.transport.connect(username=self.connect_msg["username"],
                              pkey=self.connect_msg["pkey"])

    def transtype(self,type="ssh"):
        if type=="ssh":
            self.ssh = paramiko.SSHClient()
            self.ssh._transport = self.transport
        elif type=="sftp":
            self.sftp = paramiko.SFTPClient.from_transport(self.transport)


    def exec_command(self,command):
        '''ssh执行命令'''
        self.output["stdin"], self.output["stdout"], self.output["stderr"], = self.ssh.exec_command(command)
        return self.output

    def get(self, remotepath, localpath, callback=None):
        self.sftp.get(remotepath=remotepath,localpath=localpath,callback=callback)

    def put(self,localpath, remotepath,callback=None, confirm=True):
        self.sftp.put(localpath, remotepath, callback=callback, confirm=confirm)

    def close(self):
        self.ssh.close()
        # Connection.close(self)
    @property
    def stdout(self,encode="utf8"):
        '''回去的内容有主机名加ip端口加消息'''
        stdout_dict={}
        stdout_dict["msg"]=self.output["stdout"].read().decode(encode)
        stdout_dict["hostname"]=self.connect_msg["hostname"]
        stdout_dict["ip"]=self.connect_msg["ip"]
        stdout_dict["port"]=self.connect_msg["port"]
        return stdout_dict

    @classmethod
    def from_private_key(cls,filename=None,password=None,rsa_string=None):
        '''封装了私钥对象,要么文件传入，要么字符串传入，都定义则文件优先,内部转换'''
        from io import StringIO
        if filename:
            return paramiko.RSAKey.from_private_key_file(filename,password)
        elif rsa_string:
            file_obj=StringIO(rsa_string)
            return paramiko.RSAKey(file_obj=file_obj)
```
##### paramiko对于sshkey的操作
可以生成sshkey，也可以通过私钥产生公钥
rsa公钥(ssh_rsa.pub):
```
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAmuyVsBpDrrrnlRlg/vpytrKVPmW+8pBXts8PIlIG1Df9uJ6t+Wct06Che9XeYZbcoEe+K2bODGU1PNVQOenyTz+TqG7/dJpuN2JhL3zbpH2q6iOeELX67nx09EO7CBuaMuOUP7c3MEfSWtkvJoZX0sHVc+ogu1bSvA3UtrW8UZpNtR6OzFAYfOV6Wxv6ubEGcjB3hhELM6mPFDc9lcJTLtsvYpbMd5C7jKHYtgfUHwrtmz6HDcPmv+k2hm+TVrj8GCuh87CGG/ma2r55/Uw7v3lCwwbQEMyuunMygviumkVHbwUNuaXESKxLxpiB6lZjwLz7c/uLZFfOrp2p+X754w== euewrqe@localhost.localdomain
```
通过私钥转公钥的格式
格式是{私钥的名字 私钥的内容的base64 用户名@密码}

rsa私钥
```
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAmuyVsBpDrrrnlRlg/vpytrKVPmW+8pBXts8PIlIG1Df9uJ6t
+Wct06Che9XeYZbcoEe+K2bODGU1PNVQOenyTz+TqG7/dJpuN2JhL3zbpH2q6iOe
ELX67nx09EO7CBuaMuOUP7c3MEfSWtkvJoZX0sHVc+ogu1bSvA3UtrW8UZpNtR6O
zFAYfOV6Wxv6ubEGcjB3hhELM6mPFDc9lcJTLtsvYpbMd5C7jKHYtgfUHwrtmz6H
DcPmv+k2hm+TVrj8GCuh87CGG/ma2r55/Uw7v3lCwwbQEMyuunMygviumkVHbwUN
uaXESKxLxpiB6lZjwLz7c/uLZFfOrp2p+X754wIBIwKCAQEAln9s1vTxSqbvmCdI
SCZ+EJA5NUzlYKl5x4dQh71luDZkL7AWt8NRFrlMaasvzIPsQ+3r76WyN+1JqM83
/b6w30UTGKZXPhJccFDwscm/bJdVkv4HQ27zwxnQssVz2/2OepPvGVLlJ5Y6Dxwf
LL0E2101wQ9S/yEkfCqxYQhQiceVUULGlAemsZMmqEklFK1b6B9yTGHHW+1U7CE6
pUaG2KXuqcAyQGsuw5SLSrhoWiRPn/oEdbZVMH42MmWRUijDQgic0pcM8PcdCuI6
/TM1NomuBmfsabyo0oAe6dq4X1SnKRhxl/K3DyVhxbE5JMEaGoETwyNSVR6pDhaa
XAJ4ywKBgQDKZPHWZcsYNIVIJ8SmUkKH9VuZq+NroPZGJbFsaPHWyOD5AkSKiBC7
XseVo+4Llkk77hAnWJQ5YNuHO9A/Wu1U25sV97BL69zJ6G6OVNQ7bj2/R9RX5/OK
ZS0DAqgM3RhfVhalhVVkCITjUrFdAZ+UaPK9g8i8u8VtMRTsLq3LkwKBgQDD9P6R
oPWYc3XkwldT4TSBwpZgqarZ4T75CoG6c555HYuik3XR+HsTze6GgWsCwo1Bl47i
pWxzy0B3gCe22+z8WnifQVWs3bUF35Q2dqUra+oFCzDmrPFJXncAnqQvPOxaI1U+
kF4NOBeDzgsPu1lwtPoTAayQfh/+wdriqFYqcQKBgQCKyOBYgFDAJAOfMTZja5tV
6hLtCCbqt4RNW6zcn7vGex3sk9c6a+43Vu9CCgJRFpieH5YMWf89Sbsa5y+2ajxX
cgPxzmpCsFziOPtaSMwLfstBVdNvensdEi1+aDi4XRgG2/JFnUHeMbo1eog/xplP
0u+X5VZyyeZ2wpIBChDGGwKBgGpgbO/4Ss8aG22OEiY4bPX76zvJ0ca8Gt73EzlU
tR0tS81IvE1poeYt9oOIFYUnyQZZlrWNAFwd4SrtzGqUpTiI4mUU1rztYkUEV8XL
YPpQhlqCawDhikxmex2X8rqOyXLCuT84bZIlw59SmEpeY8DXRe0PiZBEd8TPorWO
lSz7AoGAKeGfzpn8pfY17hl6ZuwxMPypiW4XfRsSh+0go1ZVcZWxaaR3Dppy/kNC
2AbMnoDdYCxU5YAya2RqbEY9REwd7ziBvsk7sJ0voRAa9DAOXEBe/ASUQ8NtxbFj
eqhZJ9MasOvbrt2sI5p+lDFBqqPhUTCyOKWpTQShIaHRRgVfhak=
-----END RSA PRIVATE KEY-----
```
私钥转公钥:
```
from _io import stringIO
text=私钥
key=paramiko.RSAKey.from_private_key(stringIO(text))     #from_private_key需要一个流 ，返回私钥的对象
key.get_name()   获取私钥类型
key.get_base64()   编码成公钥  内部是base64.encodebytes()
```
私钥是随机产生的，只要公钥符合base64转换就能匹配


私钥生成：
```
private_key_obj=paramiko.RSAKey.generate(2048)
f=stringIO()
private_key_obj.write_private_obj(f,password="123456")     #password为了生成有密码的公钥
print(f.getvalue())      #getvalue是无论是否读写都能返回数据
```




