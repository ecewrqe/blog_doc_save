### 模块
模块名+.py就是python的文件
在执行python文件时，一个是直接执行python xx.py，还有可以导入模块，写一个大的项目时通常要把多个类似的功能放到一个文件中，这个文件作为模块可以被后来的文件多次调用

##### import
import <模块名> [as <别名>]
import一个文件中只导入一次
``` python
#  root.py
def get(arg):
    print("from get:%s"%arg)
def ge2t(arg):
    print("from get2:%s"%arg)
		
# a.py
import root   #第一次导入文件，并执行了一遍这个文件，把函数，类都保存起来，第二次导入时不在执行
#导入的时候同时把作用域也导入，拿到了该模块的名字，可以调用
import root
def get(arg):
    print("from a:%s"%arg)
root.get(arg)
```

from <模块名> import <函数1,函数2...> [as  <别名>]

``` python
# a.py
from root  import get
get(arg)
def get(arg):
    print("from a:%s"%arg)
get(arg)      #函数名被本文件中的函数覆盖
from root  import get2
get2(arg)    #再次导入另一个
```
from <模块名 >import \*
``` python
# 一次性全部导入，建议不要这么使用
from root  import *
get(arg)
get2(arg)
```
\__all__可以指定*导入哪些
```python
#root.py
def get(arg):
    print("from get:%s"%arg)

def get2(arg):
    print("from get2:%s"%arg)
__all__=["get2",]

#a.py
from root import  *
get2("ddd")
from root import get
get("aaa")
```

##### __name__判断该文件作为脚本还是模块
\__name__==__main__表示作为脚本执行
\__name__==该文件名 表示作为模块

### 模块的路径
python在找寻模块的时候按照目录结构查找，像操作系统命令行查找路径一样
import root 为什么能找到root模块：
```python
import sys
sys.path   #是一个python内部环境变量的列表
'''
得出
['F:\\project\\py16session\\session04\\pack01',         #该目录就是脚本所在的目录 ，在它同级下面的模块当然第一时间就能照出来
# 以下是扩展的python模块的压缩包
'E:\\MySoft\\python35\\lib\\site-packages\\python_memcached-1.58-py3.5.egg', 
'E:\\MySoft\\python35\\lib\\site-packages\\six-1.10.0-py3.5.egg', 
'E:\\MySoft\\python35\\lib\\site-packages\\threadpool-1.3.2-py3.5.egg', 
'E:\\MySoft\\python35\\lib\\site-packages\\pip-1.4.1-py3.5.egg', 
'F:\\project\\py16session\\session04',         #该目录为项目的根目录
'E:\\MySoft\\python35\\python35.zip', 
'E:\\MySoft\\python35\\DLLs', 
'E:\\MySoft\\python35\\lib',                         #内置模块路径
'E:\\MySoft\\python35',
'E:\\MySoft\\python35\\lib\\site-packages']             #扩展模块路径
'''
# 因为是个列表，当然也可以扩展环境变量
```
**导入pack01下的root**
import pack01.root
from pack01 import root
**导入pack01下的root下的get函数**
from pack01.root import get
**错误导入**
from pack01 import root.get
import pack01.root.get
点的左边必须是一个包

### 导入包
包是一个带\__init\__.py的目录，在导入包的时候会先执行\__init\__下的内容
**导入pack01下的所有内容**
是from pack01 import * 吗？
root.get("ddd")
导入的结果是没有。
如果在\__init\__中写入:
```python
#__init__.py
print("this is __init__")
def get(arg):
    print(arg)

#a.py
from pack01 import *
get("aaa")
'''
得出结果
this is __init__
aaa
'''
# 所以*默认导入的是__init__下的所有内容，不是pack01包下的所有模块
```
\__all\__可以限制\*访问的内容，在\__init\__中也可以帮助脚本导入模块，一旦有\__all\__会执行\__all\__下所有内容，不在全部导入
```  python
#__init__.py
print("this is __init__")
def get(arg):
    print(arg)

__all__=["root"]

#a.py
from pack01 import *
root.get("aaa")
```
我有这些文件
```
top
├─pack01
│  │  other.py
│  │  root.py
│  │  __init__.py
│  │
│  └─__pycache__
│          root.cpython-35.pyc
│          __init__.cpython-35.pyc
│
├─pack02
│      tag02.py
│      __init__.py
```
执行的是other，调用pack02下的tag02这个模块
```python
from  pack02 import tag02

```