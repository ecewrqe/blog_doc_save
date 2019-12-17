## nginx
[document](https://nginx.org/en/docs/ "document")
[document2](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/ "document2")

#### load balancing负载均衡

```
http{
	upstream backend{
	}
	server{
		location / {
			proxy_pass http://backend;
		}
	}
}
```

##### algorithm
```
round robin
least connection
least time
generic hash: the given text, 
ip hash: only support http
```

other function
```
zone backend 64k;
queue 750 timeout=10s
```

limit connection:
```
weight=5
max_conn=20s
slow_start=20s
backend
down
```
stiky
```
stiky cookie
stiky learn create=, lookup=, zone=client_sessions: 2m
stiky route
```

health_check
```
health_check interval=10 passes=2 fails=3 uri=/ match=welcome

match welcome{
	status 200;
	header content-type=xxx;
	body ~ xxx
}
```


#### nginx cache缓存

#### http, web server

`autoindex on`
- rewrite

```
rewrite url_regepx dest_path (last or break)
last: stop execution of the rewrite in the current server or location
break: stop process of rewrite in current context, cancels the search the new locations

rewrite aa bb
```
rewrite response message
```
sub_filter resource destination
```

- error page
```
error_page 404 /404.html
```

- config fastcgi

php:
```
php-cgi -b 127.0.0.1:9000 # or
php-fpm -b 127.0.0.1:9000
```

```
location ~ \.php${
fastcgi_pass 127.0.0.1;
fastcgi_index index.php;
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
include fastcgi_params;
}

fastcgi_param SCRIPT_FILENAME path
$document_root => NGINX_PATH/html
$fastcgi_script_name => php_filename
```


#### location conf

```
location / {}
location ~ ^.*$ {}
location = /a/b {}

# location conf
location / {
    deny  192.168.1.1;
    allow 192.168.1.0/24;
    allow 10.1.1.0/16;
    allow 2001:0db8::/32;
    deny  all;
	
	tcp_nodelay on; # on|off
	tcp_nopush  on; # on|off
	
}

location / {
    sendfile on; # on|off
	aio on; # on|off, asynchronous IO
}

location / {
    alias 10.0.0.7/a/b/static/
}

# error_page
location / {
	error_page 404     /404.html;
	error_page 500,501    /501.html;
	
	error_page 404 =200 /a.gif;
}

location / {
	error_page 404 @fallback;
}

location @fallback {
	xxx
}
```


## uwsgi

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




## fastcgi

## tomcat


```
# path
$ ~/.bash_profile
JAVA_HOME, PATH=$JAVA_HOME/bin:$PATH, JRE_HOME
# invoke server
CATALINA_HOME/startup.sh, CATALINA_HOME/shutdown.sh
```

`chmod 755 CATALINA_HOME/*.sh`

- admin conf

`/conf/tomcat_users.xml`
`/webapps/host-manager/META-INF/context` and `/webapps/manager/META-INF/context`

- change host and connection
$CATALINA_HOME/conf/server.xml

http->servlet->jdbc

## ruby

## haproxy
















