uwsgi支持并发，
The best definition for uWSGI is “Swiss Army Knife for your network applications”.
uwsgi启动：
foobar.py
```
def application(env, start_response):
    start_response('200 OK', [('Content-Type','text/html')])
    return [b"Hello World"]
```
启动  --http 直接启动
```
uwsgi --http :9090 --wsgi-file foobar.by
```

uwsgi+nginx启动：
nginx.conf
```
location / {
include uwsgi_params;
uwsgi_pass 127.0.01:3031
}
```
uwsgi启动  --socket  给nginx代理
```
uwsgi --socket 127.0.0.1:3031 --wsgi-file uwsgi_test.py --master --processes 4 --threads 2 --stats 127.0.0.1:9191
```

使用配置文件
```
[root@example01 ~]# cat example.ini
[uwsgi]
socket = 127.0.0.1:3031
chdir = /root/
wsgi-file = uwsgi_test.py
processes = 4
threads = 2

uwsgi example.ini &
```
















