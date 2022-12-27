### pymysql
```
import pymysql
mysql=pymysql.Connection(host="10.0.0.20",port=3306,user="root",password="123456",
                   database="hostmanage",charset="utf8")
cursor=mysql.cursor()

//cursor=mysql.cursor(pymysql.cursors.DictCursor)    #��ȡ��ʱ�����ֵ�

#����
cursor.execute("select * from users")
print("��ȡ1��",cursor.fetchone())
cursor.scroll(-1)
print("��ȡ2��",cursor.fetchmany(2))
cursor.scroll(0,mode="absolute")
print("��ȡ����",cursor.fetchall())

#���
# ret=cursor.execute('''
# insert into users values(
# null,%s,%s,%s
# )
# ''',("ѩ��","e10adc3949ba59abbe56e057f20f883e",3))
user_list=[
    ("����","e10adc3949ba59abbe56e057f20f883e",2),
    ("С��","e10adc3949ba59abbe56e057f20f883e",1),
    ]
ret=cursor.executemany('''
insert into users values(
null,%s,%s,%s
)
''',user_list)
mysql.commit()

cursor.close()
```

Connection���캯��
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

##### pymysql��Connection���ع�
�ع���Ŀ����Ϊ�˰�����������󣬶����ʺ��Լ�����ķ�����Ҳ��Ϊ�˶�ģ�����ӡ��
�������Ϲ��캯��Ҫ��ô�������һ���õ���Ҳ����ô������
```
class OverrideMysql(Connection):
   #��д���췽����
    def __init__(self, host, port, username, password, database, charset="utf8"):
        '''**����ֱ�Ӱ�dict����'''
        connections.DEFAULT_CHARSET = charset
        Connection.__init__(self, host=host, port=port, user=username, passwd=password, database=database)
				
    # cursor������������ģʽ���ֵ�ģʽ��Ҫ����pymysql.cursors.DictCursor�������ҾͿ���ָ������list��dict�������ڲ����ҵ���
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
        �ƶ�ָ��
        :param value:
        :param mode: relative������ͣ�value���ƶ���������
                     absolute�����ƶ���value���ƶ���λ��
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
paramiko���ӷ�����ssh

```
import paramiko
transport = paramiko.Transport(("10.0.0.20",22))
transport.connect(username="euewrqe",password="123456")

ssh=paramiko.SSHClient()
ssh._transport = transport    #��transport

stdin,stdout,stderr=ssh.exec_command("ls")
print(stdout.read())
ssh.close()
```

```
def connect(self, hostkey=None, username='', password=None, pkey=None,
                gss_host=None, gss_auth=False, gss_kex=False, gss_deleg_creds=True):
								
```


ʹ��˽Կ
```
import paramiko
#����˽Կ
private_key=paramiko.RSAKey.from_private_key_file("./id_rsa")
#�ַ���˽Կ
from io import StringIO
file_obj=StringIO(rsa_string)
private_key=paramiko.RSAKey(file_obj=file_obj)

transport = paramiko.Transport(("10.0.0.20",22))
# ��˽Կ���û�
transport.connect(username="euewrqe",pkey=private_key)

ssh=paramiko.SSHClient()
ssh._transport = transport    #��transport

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
#����
sftp.get("remotepath","localpath")
#�ϴ�
sftp.put("localpath","remotepath")
```

##### paramiko�ع�

�ع���Ŀ�ģ�Ϊ������paramiko��ssh��sftp
```
class FastParamiko:
    #����ѡ��˽Կ������
    RSAKey = paramiko.rsakey.RSAKey
    def __init__(self,hostname,ip,port,username,password=None,pkey=None):
        '''������ʹ�����룬��˽Կʹ��˽Կ������������������'''
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
        '''sshִ������'''
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
        '''��ȥ����������������ip�˿ڼ���Ϣ'''
        stdout_dict={}
        stdout_dict["msg"]=self.output["stdout"].read().decode(encode)
        stdout_dict["hostname"]=self.connect_msg["hostname"]
        stdout_dict["ip"]=self.connect_msg["ip"]
        stdout_dict["port"]=self.connect_msg["port"]
        return stdout_dict

    @classmethod
    def from_private_key(cls,filename=None,password=None,rsa_string=None):
        '''��װ��˽Կ����,Ҫô�ļ����룬Ҫô�ַ������룬���������ļ�����,�ڲ�ת��'''
        from io import StringIO
        if filename:
            return paramiko.RSAKey.from_private_key_file(filename,password)
        elif rsa_string:
            file_obj=StringIO(rsa_string)
            return paramiko.RSAKey(file_obj=file_obj)
```