### 1.执行python脚本的两种方法
```
1，在python命令行中执行每一条语句-----通常在写程序时不会用它，只有在不知道怎么用某种方法的时候才
用命令行调试
2，python <name>.py    执行python文件
```

### 2.简述位和字节的关系
	```
	位(bit)是计算机中数据储存最小单位，一个位就是一个二进制位，只有0和1，
	字节(bytes)是计算机存储计数的一种单位，1byte=8bits
	计算机存储计数单位有：字节(B)，千字节(KB)，兆字节(MB)，...
	每一个ascii码就是一个字节
	```
### 3.简述ascii,unicode,gbk,utf-8的关系
```
ascii是最早提出的用于存储字符的编码格式，每一个ascii占一个字节0-255包含了各种控制符号，数字(48-57)，大写字母(65-90)，小写字母(97-122)，各种标点符号。
ascii码不使用于除了使用英语的国家之外的国家，就出来了unicode---万国码,每一个unicode由两个字节组成
gbk是中国境内使用的编码，全程《汉字内码扩展规范》，每个汉字两个字节
utf-8是可变长的万国码，判断ascii为一个字节，西方字母占两个字节，汉字等占三个字节
unicode和ascii在任何操作系统上都是通用的。
python2.x编码为ascii，如果要在windows上读utf-8的汉字，必须先把utf-8解码成unicode，然后再编码成gbk。
python3.x默认文件编码是utf-8，在内存中都解码成unicode，在编译python文件时，自动将文件编码转成unicode，所以不用自己解码了，想要转成其他格式的直接编码就行
。当windows的gbk的中文放进来的时候，必须windows中文默认编码有ascii和unicode和gbk，linux默认编码有ascii和unicode和utf-8，如果要通用，必须先将编码转成unicode。
```
### 4.请写出 “李杰” 分别用 utf‐8 和 gbk 编码所占的位数
```
utf8一个字占三个字节，gbk一个字占两个字节
李杰一共占六个字节:  用二进制流表示\xe6\x9d\x8e  \xe6\x9d\xb0
李杰一共占四个字节:  用二进制流表示\xc0\xee   \xbd\xdc
```
### 5.Pyhton 单行注释和多行注释分别用什么？
```
#开头为单行注释，'''-------'''开头为多行注释
多行注释可用于表示一个模块或函数或类的文档注释，用__doc__来打印
```
### 6.声明变量注意事项有那些？
```
1，声明的变量格式不能是除了字母，数字，_意外的符号
2，声明变量规范：全局变量尽量全部大写MAX_NUM=18，声明一个普通变量，在python中可以是单个word，可以是多个word加下划线分割
your_max_age=2，也可以是驼峰式：yourMaxAge=2，这种尽量别用在python
3，声明一个类：开头要用大写AgeClass
4，_或__开头的变量有特殊意义不要用在普通变量
```
### 7.如何查看变量在内存中的地址
```
id(name)
此内存地址并非真正的内存地址，是事先分配给python，由python来分配的内存
```

### 8.执行 Python 程序时，自动生成的 .pyc 文件的作用是什么
```
.pyc用于缓存模块文件，第一遍运行脚本时自动调用的模块 ，会缓存一份到pycache目录下，第二遍运行时会自动校对调用的模块内容和.pyc的内容是否不一样，不一样则删除
```

### 9.codes_group01
##### a.实现用户输入用户名和密码，当用户名为 seven 且 密码为123 时，显示登陆成功，否则登陆失败！
``` python
USER = "seven"
PWD = "123"

username=input("user:")
password=input("password:")
if username == USER and password == PWD:
    print("登陆成功!")
else:
    exit("登陆失败！")
```
##### b.实现用户输入用户名和密码，当用户名为 seven 且 密码为 123 时，显示登陆成功，否则登陆失败，失败时允许重复输入三次 
``` python
USER = "seven"
PWD = "123"

count = 3
while count>0:
    username = input("user:")
    password = input("password:")
    if username == USER and password == PWD:
        print("登陆成功!")
				break
    else:
        count -= 1
        print("登陆失败！还有%s次机会" % count)
else:
    exit("登陆失败次数过多!")
```
##### c.实现用户输入用户名和密码，当用户名为 seven 或 alex 且 密码为 123 时，显示登陆成功，否则登陆失败，失败时允许重复输入三次 
```  python
USER_GROUPS = ["seven", "alex"]
PWD = "123"
count = 3
while count>0:
    username = input("user:")
    password = input("password:")
    if username in USER_GROUPS and password == PWD:
        print("登陆成功!")
        break
    else:
        count -= 1
        print("登陆失败！还有%s次机会" % count)
else:
    exit("登陆失败次数过多!")
```
### 10.codes_group02
##### a. 使用 while 循环实现输出 2 ‐ 3 + 4 ‐ 5 + 6 ... + 100 的和 
``` python
count = 2
a = []
while count <= 100:
    if count % 2 == 0:
        a.append(count)
    else:
        a.append(-count)
    count += 1
print(a)
print(sum(a))
```
##### b. 使用 for 循环和 range 实现输出 1 ‐ 2 + 3 ‐ 4 + 5 ‐ 6 ... + 99 的和
``` python
a = [i if i%2==0 else -i for i in range(2,101)]
print(a)
print(sum(a))
```

##### c. 使用 while 循环实现输出 1，2，3，4，5， 7，8，9，11，12
``` python
count=1
while count<12:
    print(count,end=",")
    count += 1
else:
    print(count)
```

##### d. 使用 while 循环实现输出 1‐100 内的所有奇数
``` python
count = 1
a = []
while count<=100:
    if count % 2 == 1:
        a.append(count)
    count += 1
print(a)
```
##### e. 使用 while 循环实现输出 1‐100 内的所有偶数
``` python
count = 1
a = []
while count<=100:
    if count % 2 == 0:
        a.append(count)
    count += 1
print(a)
```

### 11.分别书写数字 5，10，32，7 的二进制表示
```
5:101
10:1010
32:100000
7:111
```
### 12.简述对象和 类的关系（可用比喻的手法） 
### 13.现有如下两个变量，请简述 n1 和 n2 是什么关系？
```
n1 = 123 
n2 = 123

开辟了一块空间用于存储123，n1指向了这个开辟的空间,开辟了另一块空间也存储了123，n2指向了这个开辟的空间，这两个123不是同一个123
```
### 14.现有如下两个变量，请简述 n1 和 n2 是什么关系？
```
n1 = 123456 
n2 = 123456

n1开辟了一块空间用于存储123456，n1指向了这个空间,n2开辟了另一块空间也存储了123456,n2指向了这个空间，这两个123456不是同一个123456
```
### 15.现有如下两个变量，请简述 n1 和 n2 是什么关系
```
n1 = 123456
n2 = n1
n1开辟了一块空间用于存储123456,n1指向了这个开辟的空间,n2指向也指向了这个空间，这两个123456是同一个
```
### 16.如有一下变量 n1 = 5，请使用 int 的提供的方法，得到该变量最少可以用多少个二进制位表示？
```
a=15
print(a.bit_length())
```
### 17.布尔值分别有什么
```
布尔类型bool，有两个值True(1)和False(0)
```
### 18.阅读代码，请写出执行结果
```
a = "alex" 
b = a.capitalize() 
print(a) 
print(b)

得出：
alex
Alex
```

### 19.写代码，有如下变量，请按照要求实现每个功能 name = " aleX" 
##### a. 移除 name 变量对应的值两边的空格，并输入移除有的内容
```
name.strip()
```

##### b. 判断 name 变量对应的值是否以 "al" 开头，并输出结果

```
name="alex"
if name.startswith("al"):
    print(name)
```
##### c.判断 name 变量对应的值是否以 "X" 结尾，并输出结果
```
name="alex"
if name.endswith("X"):
    print(name)
```

##### d.将 name 变量对应的值中的 “l” 替换为 “p”，并输出结果
```
name="alex"
name=name.replace("l","p")
print(name)
```
##### e.将 name 变量对应的值根据 “l” 分割，并输出结果
```
name="alex"
name=name.partition("l")
print(name)
```

##### f.上一题 e 分割之后得到值是什么类型？
```
字符串
```
##### g.将 name 变量对应的值变大写，并输出结果 
```
name="alex"
name=name.upper()
print(name)
```
##### h.将 name 变量对应的值变小写，并输出结果
```
name="alex"
name=name.lower()
print(name)
```
##### i.请输出 name 变量对应的值的第 2 个字符？
```
name="alex"
print(name[2])
```
##### j.请输出 name 变量对应的值的前 3 个字符？
```
name="alex"
print(name[0:3])
```
##### k.请输出 name 变量对应的值的后 2 个字符？
```
name="alex"
print(name[-2:])
```

##### l.请输出 name 变量对应的值中 “e” 所在索引位置
```
name="alex"
print(name.index("e"))
```
### 20.字符串是否可迭代？如可以请使用 for 循环每一个元素？
```
name="alex"
for i in name:
    print(i)
```
### 21.请用代码实现：利用下划线将列表的每一个元素拼接成字符串，li ＝ ['alex', 'eric', 'rain'] 
```
li ＝ ['alex', 'eric', 'rain'] 
print("_".join(li))
```
### 22.写代码，有如下列表，按照要求实现每一个功能
##### a.计算列表长度并输出 
```
li ＝ ['alex', 'eric', 'rain'] 
print(len(li))
```
##### b.列表中追加元素 “seven”，并输出添加后的列表
```
li.append("seven")
print(li)
```
##### c.请在列表的第 1 个位置插入元素 “Tony”，并输出添加后的列表 
```
li.insert(1,"Tony")
print(li)
```

##### d.请修改列表第 2 个位置的元素为 “Kelly”，并输出修改后的列表
```
li.insert(2,"Kelly")
```
##### e.请删除列表中的元素 “eric”，并输出修改后的列表
```
li.remove("eric")
```
##### f.请删除列表中的第 2 个元素，并输出删除的元素的值和删除元素后的列表 
```
print(li.pop(1))
```
##### g.请删除列表中的第3 个元素，并输出删除元素后的列表 
```
print(li.pop(2))
```
##### h.请删除列表中的第 2 至 4 个元素，并输出删除元素后的列表 
```
del li[1:4]
```
##### i.请将列表所有的元素反转，并输出反转后的列表 
```
li.reverse()
```
##### j.请使用 for、len、range 输出列表的索引
```
for i in range(len(li)):
    print(i)
```
##### k. 请使用 enumrate 输出列表元素和序号（序号从 100 开始）
```
for i,v in enumrate(li):
    print(i+100,v)
```

##### l.请使用 for 循环输出列表的所有元素
```
for i in li:
    print(i)
```
### 23.代码，有如下列表，请按照功能要求实现每一个功能 
```
li = ["hello", 'seven', ["mon", ["h", "kelly"], 'all'], 123, 446]
```

##### a.请输出 “Kelly”
```
print(li[2][1][1])
```
##### b.请使用索引找到 'all' 元素并将其修改为 “ALL” 
```
print(li[2][2])
```

### 写代码，有如下元组，按照要求实现每一个功能 tu ＝ ('alex', 'eric', 'rain')
##### a.计算元组长度并输出 
```
print(len(tu))
```
##### b.获取元组的第 2 个元素，并输出 
```
print(tu[1])
```
##### c.获取元组的第 1-2 个元素，并输出 
```
print(tu[0:3])
```
##### d.请使用 for 输出元组的元素
```
for i in tu:
    print(i)
```
##### e.请使用 for、len、range 输出元组的索引 
```
for i in range(len(li)):
    print(i)
```

##### f.请使用 enumrate 输出元祖元素和序号（序号从 10 开始） 
```
for i,v in enumrate(li):
    print(i+10,v)
```

### 25.有如下变量，请实现要求的功能 tu =("alex", [11, 22, {"k1": 'v1', "k2": ["age", "name"], "k3": (11,22,33)}, 44]) 
##### a.讲述元祖的特性
```
元祖是不可变的列表，元祖是常量
```
##### b.请问 tu 变量中的第一个元素 “alex” 是否可被修改？ 
```
不可变
```
##### c.请问 tu 变量中的"k2"对应的值是什么类型？是否可以被修改？如果可以，请在其中添加一个元素 “Seven”
```
k2对应一个列表，可修改
tu[1][2]["k2"].append("Seven")
```
##### d.请问 tu 变量中的"k3"对应的值是什么类型？是否可以被修改？如果可以，请在其中添加一个元素 “Seven”
```
k3对应一个元祖不可修改
```

### 26.字典dic = {'k1': "v1", "k2": "v2", "k3":[11,22,33]} 
##### a.请循环输出所有的 key 
```
for k in dic:
    print(k)
```
##### b.请循环输出所有的 value
```
for k in dic:
    print(dic[k])
```
##### c.请循环输出所有的 key 和 value 
```
for k,v in item(dic):
    print(k,v)
```
##### d.请在字典中添加一个键值对，"k4": "v4"，输出添加后的字典 
```
dic["k4"]="v4"
```
##### e.请在修改字典中 “k1” 对应的值为 “alex”，输出修改后的字典 
```
dic["k1"]="alex"
```
##### f.请在 k3 对应的值中追加一个元素 44，输出修改后的字典 
```
dic["k3"].append(44)
```
##### g.请在 k3 对应的值的第 1 个位置插入个元素 18，输出修改后的字典
```
dic["k3"].insert(1,18)
```
### 27.转换 
##### a.将字符串 s = "alex" 转换成列表 
```
s=s.split()
```
##### b.将字符串 s = "alex" 转换成元祖 
```
s=tuple(s.split())
```
##### c.将列表 li = ["alex", "seven"] 转换成元组 
```
li=tuple(li)
```
##### d.将元祖 tu = ('Alex', "seven") 转换成列表
```
li=list(tu)
```
##### e.将列表 li = ["alex", "seven"] 转换成字典且字典的 key 按照 10 开始向后递增
```
temp={}
for k,v in enumrate(li):
    temp[k+10]=v
```

### 29.转码 n="老男孩"
##### a. 将字符串转换成 utf‐8 编码的字节，并输出，然后将该字节再转换成 utf‐8 编码字符串，再输出 
```
n=bytes(n,encoding="utf8")
n=str(n,encoding="utf8")
```
##### b.将字符串转换成 gbk 编码的字节，并输出，然后将该字节再转换成 gbk 编码字符串，再输出 
```
n=bytes(n,encoding="gbk")
n=str(n,encoding="gbk")
```
### 28.求 1‐100 内的所有数的和 
```
a=[i for i in range(100)]
sum(a)
```
### 29.元素分类 

有如下值集合 [11,22,33,44,55,66,77,88,99,90]，将所有大于 66 的值保存至字典的第一个 key 中，将小于 66 的值保存至第二个 key 的值中。 
即： {'k1': 大于 66 的所有值, 'k2': 小于 66 的所有值} 
```
a=[11,22,33,44,55,66,77,88,99,90]
dic_a={"k1":[],"k2":[]}
for i in a:
    if i < 66:
       dic_a["k1"].append(i)
    elif i > 66:
       dic_a["k2"].append(i)
```
### 30.购物车
goods = [ 
{"name": "电脑", "price": 1999}, 
{"name":"鼠标", "price": 10}, 
{"name": "游艇", "price": 20}, 
{"name": "美女", "price": 998}, 
] 

### 31.基础题
##### a.计算100-300中能被3和7整除的数的和
```
a = [i if i%3==0 and i%7==0 else 0 for i in range(100,301)]
print(sum(a))

```
##### b.统计字符串中字母，数字的个数，返回结果
```
count_li=[0,0,0]
def count_letter(a):
    for i in a:
        if i.isdigit():
            count_li[0]+=1
        elif i.isupper():
            count_li[1]+=1
        elif i.islower():
            count_li[2]+=1
    return count_li
print(count_letter(count_li))
```

##### c.获取[11,22,33],[22,33,44]中相同的元素
```
a=set([11,22,33])
b=set([22,33,44])
print(a&b)
```
##### d.老男人转换成utf8字节
```
print(bytes("老男人",encoding="utf-8"))
```
##### e.计算数字的绝对值
```
abs(12)
```
##### f.简介深浅拷贝
```
只拷贝一层的叫浅拷贝
全部都拷贝的叫深拷贝
```
##### g.举例bool值为False的值
```
False,0,None,[],{}空值
```
##### h.内置函数all和any的区别
```
all指一个迭代器中只要有一个位False，返回值为false，迭代器为空时为True
any中的迭代器只要有一个为True，返回值为True，迭代器为空时为False
```
##### i.十进制12转换成其他
```
a=12
bin(12)   2
oct(12)8
hex(12)16
```
##### j.dir,help,id,type
```
dir可以列出某个对象或类的方法
help帮助文档，可以显示开头文档信息和方法
id返回内存地址
type类型
```
##### k.简述globals()和locals的作用
···
这两个是内置函数，globals用来收集所有全局变量，返回列表，locals收集局部变量，返回列表
···
##### l.三元运算的书写
```
<True> if <parameter> else <False>
```
##### m.lambda表达式
```
<func>=lambda<parameter>:<...>
func()
```
n.zip()实现：
l1=["alex",22,33,44,55]
l2=["is",22,33,44,55]
l3=["good",22,33,44,55]
l4=["guy",22,33,44,55]
获取字符串alex_is_good_guy
o."r"  and "rb"打开区别
```
r读取一个字符串，变成二进制流
rb读取一个二进制流
```
##### p.with的作用
自动close()
##### q.用with同时打开两个文件
```
with open("file1","r",encoding="utf-8"):
    with open("file2","w",encoding="utf-8"):
        xxxx
```
##### r.函数默认返回值：
None
##### s.简述函数的普通参数，关键字参数，可变参数，默认参数和注意事项：
实参：固定位置的参数，就是按照位置来传参f(a,b,c,d) 传参数也必须是f(1,2,3,4)不能多一个或少一个
关键字参数是指定形参名来传参f(a,b,c,d)  f(a=10,b=20,d=30,c=40) 这个时候与顺序无关，当形参没有默认参数和可变参数时，不能多一个或少一个参数
形参：默认参数，在调用时该参数可以不指定，默认帮你指定好，但是你也可以指定,默认参数后面不能有普通参数或可变的普通参数。f(a,b,d,c=20)
可变参数，参数数量不固定，但是必须在默认参数前面，在普通参数后面，传入的结果是一个元祖
传入关键字的可变参数，传入的结果是字典

##### t.7.88123输出后保留两位小数
```
a=88.45467
print("%0.2f"%a)
```

##### u.判读格式化是否正确
tpl1="i am {name} age {age} really {name}".format(name="seven",age=18)
tpl2="i am {0} age {1}".format(["seven",18])
```
tpl1正确 ，tpl2不正确
tpl2="i am {0} age {1}".format(*["seven",18])
```

##### python导入模块的方式：
```
import <模块名>
import 包.包...<模块名>
from 包.包.包 import <模块>
__import__(模块)
```
##### 分页
```
a=192
d,m=divmod(192,10)
if m:
    d+=1
print(d)
```
##### range
```
2.7的range返回的是列表
3.x的range返回值是迭代器
列表是可迭代，非迭代状态也能直接看内容
迭代器在迭代的时候才把内容拿出来
```
##### sys.argv
```
在命令行执行时传入参数
xx.py a b
传入了a和b
```
##### 上级目录的路径
../

##### 拼接路径
a="/sys/seven"
b="lib"
c="bin"
```
import os
print(os.path.join(os.path.join(a,b),c))
```
##### 获取随机字母
```
先获取一个随机的数字56-90and97-122
再chr()这个随机数
```
##### re的match，search，findall
match每一行从头开始找，到结尾如果没找到返回None，找到后返回
##### 正则表达式分组group
##### 简述1*2+3*4+...
先实现99*100，再把结果和97*98。。以此类推
```
1:2*1=2
2:2*1=2
3:4*3+2*1:4*3+f(2)
4:4*3+2*1:4*3+f(2)
5 6:6*5+4*3+2*1:6*5+f(4)

f(n):if n%2==1:n+=1
f(n):n*(n-1)+(n-2)*(n-3)+(n-4)*(n-5)+...  return n*(n-1)+f(n-2)
f(2):return 2*1
```

```
def f1(a):
    if a%2 == 1:
        a += 1
    if a==2:
        return a*(a-1)
    else:
        return a*(a-1)+f1(a-2)
f(100)
```

##### json和pickle
##### json中dump和load和dumps和loads作用
##### http请求本质返回什么类型
##### 书写邮箱的正则
#####  冒泡排序
```
def sort(li):
	for i in range(1,len(li)):
		for index in range(len(li)-i):
			if li[index]>li[index+1]:
				li[index],li[index+1]=swap(li[index],li[index+1])
```
##### 简述类和对象

### 读代码写结果
##### a:
a="oldboy%"
print(a)
b="oldboy %d %%"%(12,)
print(b)
```
oldboy%
oldboy 12 %
```
##### b.
def func(a1):
    return a1+100
func = lambda a1:a1+200
ret=func(10)
print(ret)
```
210
```
##### c.
def f1(arg):
    print(arg+100)
def f2(arg):
    ret=f1(arg+1)
    print(arg)
    print(ret)
ret=f2(7)
print(ret)
```
108
7
None
None
```
##### d.
def f1(arg):
    print(id(arg))
n=1111111
print(id(n))
f1(n)
两个值是否相等：
```
相等
```
##### e.
def func(arg):
    arg.append(55)
li=[11,22,33,44]
li=func(li)
print(li)
```
None
```
##### f.
NAMELIST=["alex","eric"]
def func():
    NAMELIST = 123

func()
print(NAMELIST)
```
["alex","eric"]
```
NAMELIST=["alex","eric"]
def func():
    global NAMELIST
    NAMELIST = 123
func()
print(NAMELIST)
```
123
```
NAMELIST=["alex","eric"]
def func():
    NAMELIST.append("seven")
func()
print(NAMELIST)
```
["alex","eric","seven"]
```
##### g.
r1=eval("1+10*10")
r2=exec("1+10*10")
print(r1,r2)
```
101
None
```
##### h.
import re
r=re.match("\d+","123abc").group()
print(r)
```
123
```
##### i.
def outer(func):
    def inner():
        pass
    return inner()
@outer()
def f1(arg):
    pass

支出上述代码的错误：
```
4行，return inner
5行，@outer
```
### 编程题
##### a.用filter,自定义函数获得l1中大于33的所有元素:l1=[11,22,33,44,55]
```
l1=[11,22,33,44,55]
def f1(x):
    return x>33
de01=filter(f1,l1)
for i in de01:
    print(i)
```
##### b.用filter，lambda做上述事
```
l1=[11,22,33,44,55]
de01=filter(lambda x:x>33,l1)
for i in de01:
    print(i)
```
##### c.用map和自定义函数所有奇数的元素加100:l1=[11,22,33,44,55]
```
l1=[11,22,33,44,55]
def f1(x):
    return x+100 if x%2==1 else x
de02=map(f1,l1)
for i in de02:
    print(i)
```
##### d.用map和lambda所有偶数加100:l1=[11,22,33,44,55]
```
l1=[11,22,33,44,55]
de02=map(lambda x:x+100 if x%2==0 else x,l1)
for i in de02:
    print(i)
```
##### e.for打印99乘法表
```
def f1(n):
    for i in range(1,n):
        for j in range(1,i+1):
            print("%d*%d=%d"%(j,i,i*j),end="  ")
        print()
f1(10)
```
##### f.递归实现阶乘
```
def f2(n):
    if n == 1:
        return 1
    return n*f2(n-1)
print(f2(6))
print(1*2*3*4*5*6)
```
##### 两个列表
l1=[...]    无序不重复
l2=[]     空列表
从小到大排序

```
import random
l1=[random.randrange(10) for i in range(10)]
l2=[]
'''
思路：
假设l1=[3, 7, 7, 3, 8, 1, 1, 7, 9, 8]
l2=[]

1,先把最小的拿过来l2.append(min(l1))  l2=[1]
2,循环l1，如果l1[0]小于l2的所有数，每循环一次，循环l2
3,判断l1的该数是否小于l2的每一个数，如果小于，插入到该下标，如果大于，往后继续
4，如果l2的每一个都小于l1，插到最后
'''
def sort():
    l2.append(min(l1))
    
    for i in range(len(l1)):
        for j in range(len(l2)):
            if l1[i]>l2[j]:
                continue
            elif l1[i]<=l2[j]:
                l2.insert(j,l1[i])
                break
        else:
            l2.append(l1[i])
    return l2
sort()
```
#####  装饰器
写一个装饰器，在函数前后插入before和after
```
def pack(func):
    def inner(*args, **kwargs):
        print("before")
        temp = func(*args, **kwargs)
        print("after")
        return temp

    return inner

@pack
def f1(arg):
    print(arg)

@pack
def f2(arg1, arg2):
    print(arg1, arg2)

f1("aaa")
f2("aaa", "bbb")
```

### 应用题
##### 使用requests和xml实现天气查询
http://www.webxml.com.cn/WebServices/WeatherWebService.asmx/getWeatherbyCityName?theCityName=上海

实现：
https://github.com/euewrqe/pys16/tree/master/day04/weather_search

