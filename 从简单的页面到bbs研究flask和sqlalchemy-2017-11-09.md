## flask
这是一个微型的web框架
```
from flask import Flask

app = Flask(__name__)
@app.route('/')
def hello_world():
    return 'Hello World!'
if __name__ == '__main__':
    app.run()
```

### 路由系统
```
## 传参
@app.route('/<username>')
def hello_world(username):
    return 'Hello World!---%s' % username

## 参数必须是整数
@app.route('/<int:post_id>')
## 路由映射别名，别名就是函数名
# 从函数名解析成url
from flask import url_for

url_for("hello_world", post_id=1)
#默认静态路径
print(url_for("static", filename="index.css"))

```
使用模板引擎
```
#限定请求方法
@app.route('/login', methods=["GET","POST"])
def login():
    return render_template("login.html")
```
页面模板使用jinja2引擎，和django的模板很相似。
```
<h3>bbs for flask</h3>
<p>{{ name }}</p>
<p>{{ food[0] }}</p>
<p>{{ say_hello() }}</p>
<p>======循环food=========</p>
{% for foo in food %}
<p>{{ foo }}</p>
{% endfor %}
```
### 请求信息request
```
from flask import request
```
request
```
@app.route('/login', methods=["GET","POST"])
def login():
    if request.method == "GET":
        return render_template("login.html")
    elif request.method == "POST":
        #基本表单收集
        print(request.form["username"], request.form["password"])
        f = request.files["my_pic"]    #收集文件
        print(dir(f))
        f.save("F:\\project\\py16session\\flask_bbs\\aaa.txt")
        return  redirect("/")  #from flask import redirect

# 自动保存
f.save("F:\\project\\py16session\\flask_bbs\\%s"%(secure_filename(f.filename)))
```
### cookies


### request一些参数整理
request.form  获取表单内容
request.args  获取url的数据
request.files    获取文件对象
request.cookies   获取cookies
文件对象的方法：
f.filename
f.save
f.stream
request.headers
```
Accept-Language: zh-CN,zh;q=0.8
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.100 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Cookie: sessionid=6xtgt3r51wcha3c4ro1jbg02mgm6m7zk; csrftoken=kAas6DGn6EDoE9rkmaguxwqIRQXAwGKl9PQsEux7rjuHhhggacoqaS6WXV7rJiep; framework_name=django
Connection: keep-alive
Host: 127.0.0.1:5000
Accept-Encoding: gzip, deflate, br
```

response.set_cookie(key,value,max_age,expire)
response.headers["xxx"]="xxx"

**flask-mysql小注册程序**
```
import pymysql

@app.route('/')
def index():
    connector = pymysql.Connection(host="10.0.0.20", port=3306, user="bbs", password="123456", database="bbs",
                                   charset="utf8")
    cursor = connector.cursor()
    cursor.execute("select username, password from user")
    all_data = cursor.fetchall()
    return render_template("index.html", all_data=all_data)



@app.route('/login', methods=["GET","POST"])
def login():
    if request.method == "GET":
        return render_template("login.html")
    elif request.method == "POST":
        #基本表单收集

        username = request.form.get("username")
        password = request.form.get("password")

        connector = pymysql.Connection(host="10.0.0.20", port=3306, user="bbs", password="123456", database="bbs",
                                       charset="utf8")
        cursor = connector.cursor()
        cursor.execute("insert into user(username, password) values('%s', '%s')" % (username, password))
        connector.commit()

        return  redirect("/")  #from flask import redirect
```



## jinja2

## WTForm


## 什么是ORM
ORM是对象关系映射表(Object Relational Mapping),对象-关系映射，把程序中的对象模型映射到基于SQL的关系型数据库。不需要直接和SQL语言打交道，只需要写好一个类。

python的ORM模块流行的有SQLAlchemy和django的models

## SQLAlchemy
