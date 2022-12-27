# python基本数据类型及字符串
### 引号的作用：
python可以用单引号或者双引号表示字符串，用引号来区分数字和字符串<br />
"1024"和1024和"1'02'4"：<br />
1. 加引号的1024是字符串str，可使用str相关的操作；不加引号的1024表示整数或浮点数，可进行整数或浮点数的运算。
2. int(str)会把字符串强制转换成整数(如果str中有字母或者其它字符，会报错)，str(int)会强制转换成字符串
3. 单引号和双引号没有区别，都是表示字符串，如果在字符串中表示引号，可以用不同的引号区别，也可以用转义\'
4. 三引号的第一行在前引号后面，不需要换行，如果非得换行可以用\进行换行

``` python
"Welcome! This is the documentation for Python 3.5.0,\

last updated Sep 13, 2015. "



'''\

Welcome! This is the documentation for Python 3.5.0, 

last updated Sep 13, 2015. 

'''
```

## python数据类型
python有两种数据类型：字符串和非字符串

非字符串类型有：整数int，浮点数float，复数complex-----在python2.x有长整型long

字符串又有两种类型：字符串str和字节bytes

## 字节
bytes：将普通的字符串转换成字节，如果遇到汉字需要编码成字节

``` python
>>> bytes("hello world,你好",encoding="utf-8")
b'hello world,\xe4\xbd\xa0\xe5\xa5\xbd'
>>> "hello world,你好".encode()
b'hello world,\xe4\xbd\xa0\xe5\xa5\xbd'
```

str:可以将字节解码成汉字
```python
>>> str(b"hello world,\xe4\xbd\xa0\xe5\xa5\xbd",encoding="utf-8")
'hello world,你好'
>>> b"hello world,\xe4\xbd\xa0\xe5\xa5\xbd".decode()
'hello world,你好'
```

## 字符串
#### 某段字符串

``` python
str1="my little pony"
str2="friendship is magic"
str3=str1+","+str2     #字符串普通的加法==>
str1+=","+str2        #通过这句话可得出==> +=的优先级大于+
print("str3=%s\nstr2=%s"%(str3,str1))

得出:
---my little pony,friendship is magic  #我的小马驹，友谊是魔法
```
关于编码的内容在python官方文档的The python standard library -> binary data services -> Encodings and Unicode 



#### 字符串的常用方法：
合并，查找，替换，取字符串长度,下面会一个个举例

##### 取字符串长度
len(str1)

##### 字符串查找，替换
1. index(self->str,child->str,start->int,end->int)->str
2. find(self->str,child->str,start->int,end->int)->str
3. replace(self->str,old->str,new->str,count->int/-1)->str
4. count(self->str,sub->str,start->int,end->int)->int   指出该字符串包含了多少sub中的字符


``` python
str1="my little pony,my little pony"

print(str1.index("pony"))       #默认从第零个开始找
print(str1.find("pony"))

print(str1.index("pony",11))     #从第11个开始找
print(str1.find("pony",11))
```

它俩使用方法基本一样，但是当没有找到值的时候index就会报错
```python
print(str1.index("pony",30))
```

``` python
Traceback (most recent call last):
  File "F:/project/py16session/session/session01/t1.py", line 9, in <module>
    print(str1.index("pony",30))
ValueError: substring not found
```

```python
print(str1.find("pony",30))   #返回-1
```

``` python
print(str1.replace("p","k"))
```

##### 字符串切割：
字符串也能像切割列表那样，python中基本遇到start:end:step这种，start要算进去，end不用算进去。
``` python
print(str2[0])
print(str2[5])
print(str2[2:7])
print(str2[2:])
print(str2[:7])
print(str2[:])
```

####字符串合并，分割，脱衣


恢复成之前的状态
```  python
str1=str1.strip(" ")     #将字符串左右分割

str1=str1.replace(","," ")
li1=str1.split(" ")     #以空格为分隔符
print(li1)
```
```
['my', 'little', 'pony', 'my', 'little', 'pony']
```
``` python
#恢复成之前的样子
str3=' '.join(li1)
print(str3)
```
```
my little pony my little pony
```

去掉空格或换行符：
strip(self->str,chars->str)->str

分割：
split(self->str,sep->str,maxsplit->int/-1)->list

多行分割：splitlines(self->str,keepends->bool)->list

``` python
print("""\
1,2,3,4
上勾拳
波动拳
""".splitlines())
```
```
['1,2,3,4\n', '上勾拳\n', '波动拳\n']
```
partition(self->str,sep->str)->list

##### 格式化
format(self->str,*arg)->str      
根据字符串中的{0},{1}...转换成对应的参数,
也可以根据{name},{age}...，一般用后面这种,可以无序
```
"hello {0} and {1}".format("twilight","rairy")
"hello {name1} and {name2}".format(name1="twilight",name2="rairy")
```

##### 大小写转换
1. lower(self->str)->str   大转小
2. upper(self->str)->str   小转达
3. swapcase(self->str)->str   大的转小的，小的转大的
4. capilalize(self->str)->str   头一个大写，其它全小写，
5. casefold(self->str)->str   头一个小写，其它全小写
##### 字符串填充
根据width的大小，在字符串的长度小于width的情况下其余用fillchar填充,满了就不填充
center(self->str,width->int,fillchar->str)->str   字符串居中
ljust(self->str,width->int,fillchar->str)->str   字符串在最左边
rjust(self->str,width->int,fillchar->str)->str   字符串在最右边
zfill(self->str,width->int)->str   用0填充,字符串在最右边
``` python
"kira".center(15,'★')
```

##### 匹配字符串
匹配开头:startswich(self->str,start->str,end->str)->bool

匹配结尾:endswich(self->str,start->str,end->str)->bool

##### 判断
1. isdigit(self->str)->bool   是否为数字
2. isalpha(self->str)->bool   是否为字母
3. isalnum(self->str)->bool   是否为数字和字母
4. isupper(self->str)->bool   是否大写
5. islower(self->str)->bool   是否小写
6. isidentifier(self->str)->bool   是否是一个合法变量
7. istitle(self->str)->bool    是否为标题文字，指每个单词首字母大写

### 字符串编码
1,文件在编写保存后的编码
2，python程序加载时的编码
3，控制台输出时的编码
4，浏览器输出时的编码
惯例说法：
python3加载的文件默认编码是utf8，在python程序中运行是unicode，在控制台输出：unicode
python2文件加载默认ascii，在python中运行是ascii，在控制台输出分情况：win--gbk(各国语言不一样)   linux--utf8
win默认支持gbk和unicode
linux默认支持utf8和unicode

在控制台:
```
(python3)
>>> 'fa你好'    #python3无论运行还是输出都是unicode
'fa你好'
>>> bytes('fa你好',encoding='utf8')    #编码成utf8格式的字节
b'fa\xe4\xbd\xa0\xe5\xa5\xbd'       #ascii显示  每个\x00-\xff 为1个字节
>>> bytes('你好',encoding='gbk')     #编码成gbk格式的字节
b'\xc4\xe3\xba\xc3'
#unicode可以编码成其他任何格式的字节，要想把unicode的字符串通过utf8格式编码的字节，再通过gbk解码成unicode
>>> bytes('你好',encoding='utf8')    # \xe4\xbd\xa0  \xe5\xa5\xbd
b'\xe4\xbd\xa0\xe5\xa5\xbd'
>>> str(b'\xe4\xbd\xa0\xe5\xa5\xbd',encoding='gbk')   #\xe4\xbd  \xa0\xe5  \xa5\xbd
'浣犲ソ'

unicode和utf8

```

### translate和maketrans
maketrans做映射   translate替换
```
map='abc'.maketrans('a','A')
'abc'.translate(map)
```










