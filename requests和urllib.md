### urllib的parse
```
>>> parse.splittype("ssh://127.0.0.1:8000/index/#f1")
('ssh', '//127.0.0.1:8000/index/#f1')
>>> parse.splithost("//127.0.0.1:8000/index/")
('127.0.0.1:8000', '/index/')
>>> parse.splituser("euewrqe@localhost")
('euewrqe', 'localhost')
>>> parse.splituser("euewrqe:123@localhost:8000")   #user[:passwd]@host[:port]
('euewrqe:123', 'localhost:8000')
>>> parse.splitpasswd("euewrqe:123")   #user:passwd
('euewrqe', '123')
>>> parse.splitport("localhost:8000")
('localhost', '8000')
>>> parse.splitquery("//127.0.0.1:8000/index/?p=123")  #--?
('//127.0.0.1:8000/index/', 'p=123')
>>> parse.splittag("//127.0.0.1:8000/index/#f1")
('//127.0.0.1:8000/index/', 'f1')
```
-----------------------
```
#编码
>>> parse.quote("你好")
'%E4%BD%A0%E5%A5%BD'
#解码
>>> parse.unquote("%E4%BD%A0%E5%A5%BD")
'你好'
```

```
>>> parse.urlsplit("http://127.0.0.1:8000/index/?p=22&f=21#ff")===urlparse
SplitResult(
	scheme='http', 
	netloc='127.0.0.1:8000',
	path='/index/', 
	query='p=22&f=21', 
	fragment='ff'
)
urlunparse(["http",'xxx','sxxx'])

>>> parse.urljoin("/index/","first")
'/index/first'

#unquote to string ;string to bytes
>>> parse.unquote_to_bytes("%E4%BD%A0%E5%A5%BD")
b'\xe4\xbd\xa0\xe5\xa5\xbd'

```






