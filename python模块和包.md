### ģ��
ģ����+.py����python���ļ�
��ִ��python�ļ�ʱ��һ����ֱ��ִ��python xx.py�����п��Ե���ģ�飬дһ�������Ŀʱͨ��Ҫ�Ѷ�����ƵĹ��ܷŵ�һ���ļ��У�����ļ���Ϊģ����Ա��������ļ���ε���

##### import
import <ģ����> [as <����>]
importһ���ļ���ֻ����һ��
``` python
#  root.py
def get(arg):
    print("from get:%s"%arg)
def ge2t(arg):
    print("from get2:%s"%arg)
		
# a.py
import root   #��һ�ε����ļ�����ִ����һ������ļ����Ѻ������඼�����������ڶ��ε���ʱ����ִ��
#�����ʱ��ͬʱ��������Ҳ���룬�õ��˸�ģ������֣����Ե���
import root
def get(arg):
    print("from a:%s"%arg)
root.get(arg)
```

from <ģ����> import <����1,����2...> [as  <����>]

``` python
# a.py
from root  import get
get(arg)
def get(arg):
    print("from a:%s"%arg)
get(arg)      #�����������ļ��еĺ�������
from root  import get2
get2(arg)    #�ٴε�����һ��
```
from <ģ���� >import \*
``` python
# һ����ȫ�����룬���鲻Ҫ��ôʹ��
from root  import *
get(arg)
get2(arg)
```
\__all__����ָ��*������Щ
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

##### __name__�жϸ��ļ���Ϊ�ű�����ģ��
\__name__==__main__��ʾ��Ϊ�ű�ִ��
\__name__==���ļ��� ��ʾ��Ϊģ��

### ģ���·��
python����Ѱģ���ʱ����Ŀ¼�ṹ���ң������ϵͳ�����в���·��һ��
import root Ϊʲô���ҵ�rootģ�飺
```python
import sys
sys.path   #��һ��python�ڲ������������б�
'''
�ó�
['F:\\project\\py16session\\session04\\pack01',         #��Ŀ¼���ǽű����ڵ�Ŀ¼ ������ͬ�������ģ�鵱Ȼ��һʱ������ճ���
# ��������չ��pythonģ���ѹ����
'E:\\MySoft\\python35\\lib\\site-packages\\python_memcached-1.58-py3.5.egg', 
'E:\\MySoft\\python35\\lib\\site-packages\\six-1.10.0-py3.5.egg', 
'E:\\MySoft\\python35\\lib\\site-packages\\threadpool-1.3.2-py3.5.egg', 
'E:\\MySoft\\python35\\lib\\site-packages\\pip-1.4.1-py3.5.egg', 
'F:\\project\\py16session\\session04',         #��Ŀ¼Ϊ��Ŀ�ĸ�Ŀ¼
'E:\\MySoft\\python35\\python35.zip', 
'E:\\MySoft\\python35\\DLLs', 
'E:\\MySoft\\python35\\lib',                         #����ģ��·��
'E:\\MySoft\\python35',
'E:\\MySoft\\python35\\lib\\site-packages']             #��չģ��·��
'''
# ��Ϊ�Ǹ��б���ȻҲ������չ��������
```
**����pack01�µ�root**
import pack01.root
from pack01 import root
**����pack01�µ�root�µ�get����**
from pack01.root import get
**������**
from pack01 import root.get
import pack01.root.get
�����߱�����һ����

### �����
����һ����\__init\__.py��Ŀ¼���ڵ������ʱ�����ִ��\__init\__�µ�����
**����pack01�µ���������**
��from pack01 import * ��
root.get("ddd")
����Ľ����û�С�
�����\__init\__��д��:
```python
#__init__.py
print("this is __init__")
def get(arg):
    print(arg)

#a.py
from pack01 import *
get("aaa")
'''
�ó����
this is __init__
aaa
'''
# ����*Ĭ�ϵ������__init__�µ��������ݣ�����pack01���µ�����ģ��
```
\__all\__��������\*���ʵ����ݣ���\__init\__��Ҳ���԰����ű�����ģ�飬һ����\__all\__��ִ��\__all\__���������ݣ�����ȫ������
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
������Щ�ļ�
```
top
����pack01
��  ��  other.py
��  ��  root.py
��  ��  __init__.py
��  ��
��  ����__pycache__
��          root.cpython-35.pyc
��          __init__.cpython-35.pyc
��
����pack02
��      tag02.py
��      __init__.py
```
ִ�е���other������pack02�µ�tag02���ģ��
```python
from  pack02 import tag02

```