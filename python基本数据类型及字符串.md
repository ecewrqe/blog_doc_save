# python�����������ͼ��ַ���
### ���ŵ����ã�
python�����õ����Ż���˫���ű�ʾ�ַ��������������������ֺ��ַ���<br />
"1024"��1024��"1'02'4"��<br />
1. �����ŵ�1024���ַ���str����ʹ��str��صĲ������������ŵ�1024��ʾ�����򸡵������ɽ��������򸡵��������㡣
2. int(str)����ַ���ǿ��ת��������(���str������ĸ���������ַ����ᱨ��)��str(int)��ǿ��ת�����ַ���
3. �����ź�˫����û�����𣬶��Ǳ�ʾ�ַ�����������ַ����б�ʾ���ţ������ò�ͬ����������Ҳ������ת��\'
4. �����ŵĵ�һ����ǰ���ź��棬����Ҫ���У�����ǵû��п�����\���л���

``` python
"Welcome! This is the documentation for Python 3.5.0,\

last updated Sep 13, 2015. "



'''\

Welcome! This is the documentation for Python 3.5.0, 

last updated Sep 13, 2015. 

'''
```

## python��������
python�������������ͣ��ַ����ͷ��ַ���

���ַ��������У�����int��������float������complex-----��python2.x�г�����long

�ַ��������������ͣ��ַ���str���ֽ�bytes

## �ֽ�
bytes������ͨ���ַ���ת�����ֽڣ��������������Ҫ������ֽ�

``` python
>>> bytes("hello world,���",encoding="utf-8")
b'hello world,\xe4\xbd\xa0\xe5\xa5\xbd'
>>> "hello world,���".encode()
b'hello world,\xe4\xbd\xa0\xe5\xa5\xbd'
```

str:���Խ��ֽڽ���ɺ���
```python
>>> str(b"hello world,\xe4\xbd\xa0\xe5\xa5\xbd",encoding="utf-8")
'hello world,���'
>>> b"hello world,\xe4\xbd\xa0\xe5\xa5\xbd".decode()
'hello world,���'
```

## �ַ���
#### ĳ���ַ���

``` python
str1="my little pony"
str2="friendship is magic"
str3=str1+","+str2     #�ַ�����ͨ�ļӷ�==>
str1+=","+str2        #ͨ����仰�ɵó�==> +=�����ȼ�����+
print("str3=%s\nstr2=%s"%(str3,str1))

�ó�:
---my little pony,friendship is magic  #�ҵ�С��ԣ�������ħ��
```
���ڱ����������python�ٷ��ĵ���The python standard library -> binary data services -> Encodings and Unicode 



#### �ַ����ĳ��÷�����
�ϲ������ң��滻��ȡ�ַ�������,�����һ��������

##### ȡ�ַ�������
len(str1)

##### �ַ������ң��滻
1. index(self->str,child->str,start->int,end->int)->str
2. find(self->str,child->str,start->int,end->int)->str
3. replace(self->str,old->str,new->str,count->int/-1)->str
4. count(self->str,sub->str,start->int,end->int)->int   ָ�����ַ��������˶���sub�е��ַ�


``` python
str1="my little pony,my little pony"

print(str1.index("pony"))       #Ĭ�ϴӵ������ʼ��
print(str1.find("pony"))

print(str1.index("pony",11))     #�ӵ�11����ʼ��
print(str1.find("pony",11))
```

����ʹ�÷�������һ�������ǵ�û���ҵ�ֵ��ʱ��index�ͻᱨ��
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
print(str1.find("pony",30))   #����-1
```


``` python
print(str1.replace("p","k"))
```
``` C
my little kony,my little kony
```
##### �ַ����и
�ַ���Ҳ�����и��б�������python�л�������start:end:step���֣�startҪ���ȥ��end�������ȥ��
``` python
print(str2[0])
print(str2[5])
print(str2[2:7])
print(str2[2:])
print(str2[:7])
print(str2[:])
```

####�ַ����ϲ����ָ����


�ָ���֮ǰ��״̬
```  python
str1=str1.strip(" ")     #���ַ������ҷָ�

str1=str1.replace(","," ")
li1=str1.split(" ")     #�Կո�Ϊ�ָ���
print(li1)
```
```
['my', 'little', 'pony', 'my', 'little', 'pony']
```
``` python
#�ָ���֮ǰ������
str3=' '.join(li1)
print(str3)
```
```
my little pony my little pony
```

ȥ���ո���з���
strip(self->str,chars->str)->str

�ָ
split(self->str,sep->str,maxsplit->int/-1)->list

���зָsplitlines(self->str,keepends->bool)->list

``` python
print("""\
1,2,3,4
�Ϲ�ȭ
����ȭ
""".splitlines())
```
```
['1,2,3,4\n', '�Ϲ�ȭ\n', '����ȭ\n']
```
partition(self->str,sep->str)->list

##### ��ʽ��
format(self->str,*arg)->str      
�����ַ����е�{0},{1}...ת���ɶ�Ӧ�Ĳ���,
Ҳ���Ը���{name},{age}...��һ���ú�������,��������
```
"hello {0} and {1}".format("twilight","rairy")
"hello {name1} and {name2}".format(name1="twilight",name2="rairy")
```

##### ��Сдת��
1. lower(self->str)->str   ��תС
2. upper(self->str)->str   Сת��
3. swapcase(self->str)->str   ���תС�ģ�С��ת���
4. capilalize(self->str)->str   ͷһ����д������ȫСд��
5. casefold(self->str)->str   ͷһ��Сд������ȫСд
##### �ַ������
����width�Ĵ�С�����ַ����ĳ���С��width�������������fillchar���,���˾Ͳ����
center(self->str,width->int,fillchar->str)->str   �ַ�������
ljust(self->str,width->int,fillchar->str)->str   �ַ����������
rjust(self->str,width->int,fillchar->str)->str   �ַ��������ұ�
zfill(self->str,width->int)->str   ��0���,�ַ��������ұ�
``` python
"kira".center(15,'��')
```

##### ƥ���ַ���
ƥ�俪ͷ:startswich(self->str,start->str,end->str)->bool

ƥ���β:endswich(self->str,start->str,end->str)->bool

##### �ж�
1. isdigit(self->str)->bool   �Ƿ�Ϊ����
2. isalpha(self->str)->bool   �Ƿ�Ϊ��ĸ
3. isalnum(self->str)->bool   �Ƿ�Ϊ���ֺ���ĸ
4. isupper(self->str)->bool   �Ƿ��д
5. islower(self->str)->bool   �Ƿ�Сд
6. isidentifier(self->str)->bool   �Ƿ���һ���Ϸ�����
7. istitle(self->str)->bool    �Ƿ�Ϊ�������֣�ָÿ����������ĸ��д

