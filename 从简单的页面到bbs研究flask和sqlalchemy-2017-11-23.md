```
## flask
这是一个微型的web框架

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

## flask 蓝图
总的FLASK看作一个项目，蓝图看作一个app，用户请求一个app要先经过总项目，然后到达app，所以在总的flask中注册app的蓝图


1，创建app
2，创建蓝图
3，注册蓝图
```
import app01
app.register_blueprint(app01.bp)
```

> app01/__init__.py

```
from flask import Blueprint
bp = Blueprint('user', __name__, url_prefix='/user')
bp = Blueprint('user', \_\_name\_\_, url_prefix='/user')

@bp.route('/')
def index():
    return "Users Index page"
```

## WTForms
这是一个form表单验证组件
```
pip install wtforms  -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
```
简单实现
```
# 表单
from wtforms import Form, StringField, validators, DateTimeField, TextAreaField

class UserRegForm(Form):
    username = StringField('Username', [validators.Length(min=4, max=25)])
    birthday = DateTimeField('Birthday', format="%m/%d/%Y")
    introduce = TextAreaField('Introduce')


# 视图
@app.route('/reg', methods=["GET","POST"])
def reg():
    if request.method == "GET":
        form = UserRegForm()
    else:
        form = UserRegForm(request.form)
        print(form.validate())
        print(form.errors)

    return render_template('reg.html', form=form)

#模板
<form action="/reg"  method="post">
    <div>
        <label for="{{ form.username.id }}">用户名:</label>
        {{ form.username }}
    </div>
    <div>
        <label for="{{ form.birthday.id }}">生日:</label>
        {{ form.birthday }}
    </div>
    <div>
        <label for="{{ form.introduce.id }}">说明:</label>
        {{ form.introduce }}
    </div>
    <input type="submit" value="submit" />
</form>
```
form.errors错误提示：
```
{'email': ['Invalid email address.'], 'username': ['Field must be between 4 and 25 characters long.']}
```
表单Field基类构造函数
```
def __init__(self, label=None, validators=None, filters=tuple(),
                 description='', id=None, default=None, widget=None,
                 render_kw=None, _form=None, _name=None, _prefix='',
                 _translations=None, _meta=None):
```


## SQLAlchemy
### 什么是ORM
ORM是对象关系映射表(Object Relational Mapping),对象-关系映射，把程序中的对象模型映射到基于SQL的关系型数据库。

python的ORM模块流行的有SQLAlchemy和django的models

### 用法
```
engine = create_engine("mysql+pymysql://bbs:123456@10.0.0.20/bbs")

result = engine.execute("select username, password from user")
print(result.fetchall())
```
url规则：
`数据库类型+模块://用户名:密码@主机名:端口/数据库名`
支持事务
```
with engine.begin() as stom:
    stom.execute("insert into user(username, password) values('%s', '%s')" % ("gouzi", "gouzi111"))
    stom.execute("insert into user(username, password) values('%s', '%s')" % ("shao", "shao111"))
```
创建表
```
metaData = MetaData()
color = Table('color', metaData,
              Column('id', Integer, primary_key=True),
              Column('name', String(20))
              )
user = Table('user', metaData,
             Column('id', Integer, primary_key=True),
             Column('username', String(20)),
             Column('password', String(20))
             )

engine = create_engine("mysql+pymysql://bbs:123456@10.0.0.20/bbs")

metaData.create_all(engine)    # 创建所有表
```
orm类
```
Base = declarative_base()

class User(Base):
    '''
    用户表
    '''
    __tablename__ = "user"
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(20), nullable=False)
    password = Column(String(12), nullable=False)
    email = Column(String(20), nullable=False)

class Post(Base):
    '''
    帖子表
    '''
    __tablename__ = "post"
    id = Column(Integer, primary_key=True, autoincrement=True)
    post_title = Column(String(15), nullable=False)
    post_content = Column(Text(), nullable=False)
    user = Column(ForeignKey('user.id'), nullable=False)

class Command(Base):
    '''
    评论表
    '''
    __tablename__ = "command"
    id = Column(Integer, primary_key=True, autoincrement=True)
    command_content = Column(Text(), nullable=False)
    user = Column(ForeignKey('user.id'), nullable=False)
    post = Column(ForeignKey('post.id'), nullable=False)
#创建表
Base.metadata.create_all(engine)
```

会话操作
```
Session = sessionmaker(bind=engine)
session = Session()
session.add_all([
    User(username="alex", password="123456"),
    User(username="ga", password="ga222"),
])
session.commit()
```
获取所有数据
```
res = session.query(User)   # res返回的是一个User对象的列表，循环它，用.或dict就能拿到数据
for query in res.all():
    print(query.__dict__)
```
条件搜索
```
res = session.query(User).filter(User.id>2)
res = session.query(User).filter(User.username == "alex")
res = session.query(User).filter(User.username.in_(["alex", "shao"]))
res = session.query(User).filter(User.username.like("%le%"))
## and_和or_
res = session.query(User).filter(or_(User.username.like("%le%"), User.username.like("gouzi%")))
```
更新
```
session.query(Emp).filter(Emp.ename == "jones").update({"job": "clerk"})
session.commit()
```
删除
```
session.query(Emp).filter(Emp.ename == "blake").delete()
session.commit()
```
Column类的传参
```
name:传一个名字
type:类型
autoincrement
default
doc
key
index
nullable
primary_key
unique
```
外键
```
ForeignKey('user.id')   user是指表名
```

choice的用法：
```
class User(Base):
    TYPES = [
        (u'admin', u'Admin'),
        (u'regular-user', u'Regular user')
    ]

    __tablename__ = 'user'
    id = Column(sa.Integer, primary_key=True)
    name = Column(sa.Unicode(255))
    type = Column(ChoiceType(TYPES))
```

### 不使用蓝本的情况下做一个bbs










