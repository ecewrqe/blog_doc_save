### 1.ִ��python�ű������ַ���
```
1����python��������ִ��ÿһ�����-----ͨ����д����ʱ����������ֻ���ڲ�֪����ô��ĳ�ַ�����ʱ���
�������е���
2��python <name>.py    ִ��python�ļ�
```

### 2.����λ���ֽڵĹ�ϵ
	```
	λ(bit)�Ǽ���������ݴ�����С��λ��һ��λ����һ��������λ��ֻ��0��1��
	�ֽ�(bytes)�Ǽ�����洢������һ�ֵ�λ��1byte=8bits
	������洢������λ�У��ֽ�(B)��ǧ�ֽ�(KB)�����ֽ�(MB)��...
	ÿһ��ascii�����һ���ֽ�
	```
### 3.����ascii,unicode,gbk,utf-8�Ĺ�ϵ
```
ascii��������������ڴ洢�ַ��ı����ʽ��ÿһ��asciiռһ���ֽ�0-255�����˸��ֿ��Ʒ��ţ�����(48-57)����д��ĸ(65-90)��Сд��ĸ(97-122)�����ֱ����š�
ascii�벻ʹ���ڳ���ʹ��Ӣ��Ĺ���֮��Ĺ��ң��ͳ�����unicode---�����,ÿһ��unicode�������ֽ����
gbk���й�����ʹ�õı��룬ȫ�̡�����������չ�淶����ÿ�����������ֽ�
utf-8�ǿɱ䳤������룬�ж�asciiΪһ���ֽڣ�������ĸռ�����ֽڣ����ֵ�ռ�����ֽ�
unicode��ascii���κβ���ϵͳ�϶���ͨ�õġ�
python2.x����Ϊascii�����Ҫ��windows�϶�utf-8�ĺ��֣������Ȱ�utf-8�����unicode��Ȼ���ٱ����gbk��
python3.xĬ���ļ�������utf-8�����ڴ��ж������unicode���ڱ���python�ļ�ʱ���Զ����ļ�����ת��unicode�����Բ����Լ������ˣ���Ҫת��������ʽ��ֱ�ӱ������
����windows��gbk�����ķŽ�����ʱ�򣬱���windows����Ĭ�ϱ�����ascii��unicode��gbk��linuxĬ�ϱ�����ascii��unicode��utf-8�����Ҫͨ�ã������Ƚ�����ת��unicode��
```
### 4.��д�� ����ܡ� �ֱ��� utf�\8 �� gbk ������ռ��λ��
```
utf8һ����ռ�����ֽڣ�gbkһ����ռ�����ֽ�
���һ��ռ�����ֽ�:  �ö���������ʾ\xe6\x9d\x8e  \xe6\x9d\xb0
���һ��ռ�ĸ��ֽ�:  �ö���������ʾ\xc0\xee   \xbd\xdc
```
### 5.Pyhton ����ע�ͺͶ���ע�ͷֱ���ʲô��
```
#��ͷΪ����ע�ͣ�'''-------'''��ͷΪ����ע��
����ע�Ϳ����ڱ�ʾһ��ģ�����������ĵ�ע�ͣ���__doc__����ӡ
```
### 6.��������ע����������Щ��
```
1�������ı�����ʽ�����ǳ�����ĸ�����֣�_����ķ���
2�����������淶��ȫ�ֱ�������ȫ����дMAX_NUM=18������һ����ͨ��������python�п����ǵ���word�������Ƕ��word���»��߷ָ�
your_max_age=2��Ҳ�������շ�ʽ��yourMaxAge=2�����־���������python
3������һ���ࣺ��ͷҪ�ô�дAgeClass
4��_��__��ͷ�ı������������岻Ҫ������ͨ����
```
### 7.��β鿴�������ڴ��еĵ�ַ
```
id(name)
���ڴ��ַ�����������ڴ��ַ�������ȷ����python����python��������ڴ�
```

### 8.ִ�� Python ����ʱ���Զ����ɵ� .pyc �ļ���������ʲô
```
.pyc���ڻ���ģ���ļ�����һ�����нű�ʱ�Զ����õ�ģ�� ���Ỻ��һ�ݵ�pycacheĿ¼�£��ڶ�������ʱ���Զ�У�Ե��õ�ģ�����ݺ�.pyc�������Ƿ�һ������һ����ɾ��
```

### 9.codes_group01
##### a.ʵ���û������û��������룬���û���Ϊ seven �� ����Ϊ123 ʱ����ʾ��½�ɹ��������½ʧ�ܣ�
``` python
USER = "seven"
PWD = "123"

username=input("user:")
password=input("password:")
if username == USER and password == PWD:
    print("��½�ɹ�!")
else:
    exit("��½ʧ�ܣ�")
```
##### b.ʵ���û������û��������룬���û���Ϊ seven �� ����Ϊ 123 ʱ����ʾ��½�ɹ��������½ʧ�ܣ�ʧ��ʱ�����ظ��������� 
``` python
USER = "seven"
PWD = "123"

count = 3
while count>0:
    username = input("user:")
    password = input("password:")
    if username == USER and password == PWD:
        print("��½�ɹ�!")
				break
    else:
        count -= 1
        print("��½ʧ�ܣ�����%s�λ���" % count)
else:
    exit("��½ʧ�ܴ�������!")
```
##### c.ʵ���û������û��������룬���û���Ϊ seven �� alex �� ����Ϊ 123 ʱ����ʾ��½�ɹ��������½ʧ�ܣ�ʧ��ʱ�����ظ��������� 
```  python
USER_GROUPS = ["seven", "alex"]
PWD = "123"
count = 3
while count>0:
    username = input("user:")
    password = input("password:")
    if username in USER_GROUPS and password == PWD:
        print("��½�ɹ�!")
        break
    else:
        count -= 1
        print("��½ʧ�ܣ�����%s�λ���" % count)
else:
    exit("��½ʧ�ܴ�������!")
```
### 10.codes_group02
##### a. ʹ�� while ѭ��ʵ����� 2 �\ 3 + 4 �\ 5 + 6 ... + 100 �ĺ� 
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
##### b. ʹ�� for ѭ���� range ʵ����� 1 �\ 2 + 3 �\ 4 + 5 �\ 6 ... + 99 �ĺ�
``` python
a = [i if i%2==0 else -i for i in range(2,101)]
print(a)
print(sum(a))
```

##### c. ʹ�� while ѭ��ʵ����� 1��2��3��4��5�� 7��8��9��11��12
``` python
count=1
while count<12:
    print(count,end=",")
    count += 1
else:
    print(count)
```

##### d. ʹ�� while ѭ��ʵ����� 1�\100 �ڵ���������
``` python
count = 1
a = []
while count<=100:
    if count % 2 == 1:
        a.append(count)
    count += 1
print(a)
```
##### e. ʹ�� while ѭ��ʵ����� 1�\100 �ڵ�����ż��
``` python
count = 1
a = []
while count<=100:
    if count % 2 == 0:
        a.append(count)
    count += 1
print(a)
```

### 11.�ֱ���д���� 5��10��32��7 �Ķ����Ʊ�ʾ
```
5:101
10:1010
32:100000
7:111
```
### 12.��������� ��Ĺ�ϵ�����ñ������ַ��� 
### 13.����������������������� n1 �� n2 ��ʲô��ϵ��
```
n1 = 123 
n2 = 123

������һ��ռ����ڴ洢123��n1ָ����������ٵĿռ�,��������һ��ռ�Ҳ�洢��123��n2ָ����������ٵĿռ䣬������123����ͬһ��123
```
### 14.����������������������� n1 �� n2 ��ʲô��ϵ��
```
n1 = 123456 
n2 = 123456

n1������һ��ռ����ڴ洢123456��n1ָ��������ռ�,n2��������һ��ռ�Ҳ�洢��123456,n2ָ��������ռ䣬������123456����ͬһ��123456
```
### 15.����������������������� n1 �� n2 ��ʲô��ϵ
```
n1 = 123456
n2 = n1
n1������һ��ռ����ڴ洢123456,n1ָ����������ٵĿռ�,n2ָ��Ҳָ��������ռ䣬������123456��ͬһ��
```
### 16.����һ�±��� n1 = 5����ʹ�� int ���ṩ�ķ������õ��ñ������ٿ����ö��ٸ�������λ��ʾ��
```
a=15
print(a.bit_length())
```
### 17.����ֵ�ֱ���ʲô
```
��������bool��������ֵTrue(1)��False(0)
```
### 18.�Ķ����룬��д��ִ�н��
```
a = "alex" 
b = a.capitalize() 
print(a) 
print(b)

�ó���
alex
Alex
```

### 19.д���룬�����±������밴��Ҫ��ʵ��ÿ������ name = " aleX" 
##### a. �Ƴ� name ������Ӧ��ֵ���ߵĿո񣬲������Ƴ��е�����
```
name.strip()
```

##### b. �ж� name ������Ӧ��ֵ�Ƿ��� "al" ��ͷ����������

```
name="alex"
if name.startswith("al"):
    print(name)
```
##### c.�ж� name ������Ӧ��ֵ�Ƿ��� "X" ��β����������
```
name="alex"
if name.endswith("X"):
    print(name)
```

##### d.�� name ������Ӧ��ֵ�е� ��l�� �滻Ϊ ��p������������
```
name="alex"
name=name.replace("l","p")
print(name)
```
##### e.�� name ������Ӧ��ֵ���� ��l�� �ָ��������
```
name="alex"
name=name.partition("l")
print(name)
```

##### f.��һ�� e �ָ�֮��õ�ֵ��ʲô���ͣ�
```
�ַ���
```
##### g.�� name ������Ӧ��ֵ���д���������� 
```
name="alex"
name=name.upper()
print(name)
```
##### h.�� name ������Ӧ��ֵ��Сд����������
```
name="alex"
name=name.lower()
print(name)
```
##### i.����� name ������Ӧ��ֵ�ĵ� 2 ���ַ���
```
name="alex"
print(name[2])
```
##### j.����� name ������Ӧ��ֵ��ǰ 3 ���ַ���
```
name="alex"
print(name[0:3])
```
##### k.����� name ������Ӧ��ֵ�ĺ� 2 ���ַ���
```
name="alex"
print(name[-2:])
```

##### l.����� name ������Ӧ��ֵ�� ��e�� ��������λ��
```
name="alex"
print(name.index("e"))
```
### 20.�ַ����Ƿ�ɵ������������ʹ�� for ѭ��ÿһ��Ԫ�أ�
```
name="alex"
for i in name:
    print(i)
```
### 21.���ô���ʵ�֣������»��߽��б��ÿһ��Ԫ��ƴ�ӳ��ַ�����li �� ['alex', 'eric', 'rain'] 
```
li �� ['alex', 'eric', 'rain'] 
print("_".join(li))
```
### 22.д���룬�������б�����Ҫ��ʵ��ÿһ������
##### a.�����б��Ȳ���� 
```
li �� ['alex', 'eric', 'rain'] 
print(len(li))
```
##### b.�б���׷��Ԫ�� ��seven�����������Ӻ���б�
```
li.append("seven")
print(li)
```
##### c.�����б�ĵ� 1 ��λ�ò���Ԫ�� ��Tony�����������Ӻ���б� 
```
li.insert(1,"Tony")
print(li)
```

##### d.���޸��б�� 2 ��λ�õ�Ԫ��Ϊ ��Kelly����������޸ĺ���б�
```
li.insert(2,"Kelly")
```
##### e.��ɾ���б��е�Ԫ�� ��eric����������޸ĺ���б�
```
li.remove("eric")
```
##### f.��ɾ���б��еĵ� 2 ��Ԫ�أ������ɾ����Ԫ�ص�ֵ��ɾ��Ԫ�غ���б� 
```
print(li.pop(1))
```
##### g.��ɾ���б��еĵ�3 ��Ԫ�أ������ɾ��Ԫ�غ���б� 
```
print(li.pop(2))
```
##### h.��ɾ���б��еĵ� 2 �� 4 ��Ԫ�أ������ɾ��Ԫ�غ���б� 
```
del li[1:4]
```
##### i.�뽫�б����е�Ԫ�ط�ת���������ת����б� 
```
li.reverse()
```
##### j.��ʹ�� for��len��range ����б������
```
for i in range(len(li)):
    print(i)
```
##### k. ��ʹ�� enumrate ����б�Ԫ�غ���ţ���Ŵ� 100 ��ʼ��
```
for i,v in enumrate(li):
    print(i+100,v)
```

##### l.��ʹ�� for ѭ������б������Ԫ��
```
for i in li:
    print(i)
```
### 23.���룬�������б��밴�չ���Ҫ��ʵ��ÿһ������ 
```
li = ["hello", 'seven', ["mon", ["h", "kelly"], 'all'], 123, 446]
```

##### a.����� ��Kelly��
```
print(li[2][1][1])
```
##### b.��ʹ�������ҵ� 'all' Ԫ�ز������޸�Ϊ ��ALL�� 
```
print(li[2][2])
```

### д���룬������Ԫ�飬����Ҫ��ʵ��ÿһ������ tu �� ('alex', 'eric', 'rain')
##### a.����Ԫ�鳤�Ȳ���� 
```
print(len(tu))
```
##### b.��ȡԪ��ĵ� 2 ��Ԫ�أ������ 
```
print(tu[1])
```
##### c.��ȡԪ��ĵ� 1-2 ��Ԫ�أ������ 
```
print(tu[0:3])
```
##### d.��ʹ�� for ���Ԫ���Ԫ��
```
for i in tu:
    print(i)
```
##### e.��ʹ�� for��len��range ���Ԫ������� 
```
for i in range(len(li)):
    print(i)
```

##### f.��ʹ�� enumrate ���Ԫ��Ԫ�غ���ţ���Ŵ� 10 ��ʼ�� 
```
for i,v in enumrate(li):
    print(i+10,v)
```

### 25.�����±�������ʵ��Ҫ��Ĺ��� tu =("alex", [11, 22, {"k1": 'v1', "k2": ["age", "name"], "k3": (11,22,33)}, 44]) 
##### a.����Ԫ�������
```
Ԫ���ǲ��ɱ���б�Ԫ���ǳ���
```
##### b.���� tu �����еĵ�һ��Ԫ�� ��alex�� �Ƿ�ɱ��޸ģ� 
```
���ɱ�
```
##### c.���� tu �����е�"k2"��Ӧ��ֵ��ʲô���ͣ��Ƿ���Ա��޸ģ�������ԣ������������һ��Ԫ�� ��Seven��
```
k2��Ӧһ���б����޸�
tu[1][2]["k2"].append("Seven")
```
##### d.���� tu �����е�"k3"��Ӧ��ֵ��ʲô���ͣ��Ƿ���Ա��޸ģ�������ԣ������������һ��Ԫ�� ��Seven��
```
k3��Ӧһ��Ԫ�治���޸�
```

### 26.�ֵ�dic = {'k1': "v1", "k2": "v2", "k3":[11,22,33]} 
##### a.��ѭ��������е� key 
```
for k in dic:
    print(k)
```
##### b.��ѭ��������е� value
```
for k in dic:
    print(dic[k])
```
##### c.��ѭ��������е� key �� value 
```
for k,v in item(dic):
    print(k,v)
```
##### d.�����ֵ������һ����ֵ�ԣ�"k4": "v4"�������Ӻ���ֵ� 
```
dic["k4"]="v4"
```
##### e.�����޸��ֵ��� ��k1�� ��Ӧ��ֵΪ ��alex��������޸ĺ���ֵ� 
```
dic["k1"]="alex"
```
##### f.���� k3 ��Ӧ��ֵ��׷��һ��Ԫ�� 44������޸ĺ���ֵ� 
```
dic["k3"].append(44)
```
##### g.���� k3 ��Ӧ��ֵ�ĵ� 1 ��λ�ò����Ԫ�� 18������޸ĺ���ֵ�
```
dic["k3"].insert(1,18)
```
### 27.ת�� 
##### a.���ַ��� s = "alex" ת�����б� 
```
s=s.split()
```
##### b.���ַ��� s = "alex" ת����Ԫ�� 
```
s=tuple(s.split())
```
##### c.���б� li = ["alex", "seven"] ת����Ԫ�� 
```
li=tuple(li)
```
##### d.��Ԫ�� tu = ('Alex', "seven") ת�����б�
```
li=list(tu)
```
##### e.���б� li = ["alex", "seven"] ת�����ֵ����ֵ�� key ���� 10 ��ʼ������
```
temp={}
for k,v in enumrate(li):
    temp[k+10]=v
```

### 29.ת�� n="���к�"
##### a. ���ַ���ת���� utf�\8 ������ֽڣ��������Ȼ�󽫸��ֽ���ת���� utf�\8 �����ַ���������� 
```
n=bytes(n,encoding="utf8")
n=str(n,encoding="utf8")
```
##### b.���ַ���ת���� gbk ������ֽڣ��������Ȼ�󽫸��ֽ���ת���� gbk �����ַ���������� 
```
n=bytes(n,encoding="gbk")
n=str(n,encoding="gbk")
```
### 28.�� 1�\100 �ڵ��������ĺ� 
```
a=[i for i in range(100)]
sum(a)
```
### 29.Ԫ�ط��� 

������ֵ���� [11,22,33,44,55,66,77,88,99,90]�������д��� 66 ��ֵ�������ֵ�ĵ�һ�� key �У���С�� 66 ��ֵ�������ڶ��� key ��ֵ�С� 
���� {'k1': ���� 66 ������ֵ, 'k2': С�� 66 ������ֵ} 
```
a=[11,22,33,44,55,66,77,88,99,90]
dic_a={"k1":[],"k2":[]}
for i in a:
    if i < 66:
       dic_a["k1"].append(i)
    elif i > 66:
       dic_a["k2"].append(i)
```
### 30.���ﳵ
goods = [ 
{"name": "����", "price": 1999}, 
{"name":"���", "price": 10}, 
{"name": "��ͧ", "price": 20}, 
{"name": "��Ů", "price": 998}, 
] 

### 31.������
##### a.����100-300���ܱ�3��7���������ĺ�
```
a = [i if i%3==0 and i%7==0 else 0 for i in range(100,301)]
print(sum(a))

```
##### b.ͳ���ַ�������ĸ�����ֵĸ��������ؽ��
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

##### c.��ȡ[11,22,33],[22,33,44]����ͬ��Ԫ��
```
a=set([11,22,33])
b=set([22,33,44])
print(a&b)
```
##### d.������ת����utf8�ֽ�
```
print(bytes("������",encoding="utf-8"))
```
##### e.�������ֵľ���ֵ
```
abs(12)
```
##### f.�����ǳ����
```
ֻ����һ��Ľ�ǳ����
ȫ���������Ľ����
```
##### g.����boolֵΪFalse��ֵ
```
False,0,None,[],{}��ֵ
```
##### h.���ú���all��any������
```
allָһ����������ֻҪ��һ��λFalse������ֵΪfalse��������Ϊ��ʱΪTrue
any�еĵ�����ֻҪ��һ��ΪTrue������ֵΪTrue��������Ϊ��ʱΪFalse
```
##### i.ʮ����12ת��������
```
a=12
bin(12)   2
oct(12)8
hex(12)16
```
##### j.dir,help,id,type
```
dir�����г�ĳ���������ķ���
help�����ĵ���������ʾ��ͷ�ĵ���Ϣ�ͷ���
id�����ڴ��ַ
type����
```
##### k.����globals()��locals������
������
�����������ú�����globals�����ռ�����ȫ�ֱ����������б�locals�ռ��ֲ������������б�
������
##### l.��Ԫ�������д
```
<True> if <parameter> else <False>
```
##### m.lambda���ʽ
```
<func>=lambda<parameter>:<...>
func()
```
n.zip()ʵ�֣�
l1=["alex",22,33,44,55]
l2=["is",22,33,44,55]
l3=["good",22,33,44,55]
l4=["guy",22,33,44,55]
��ȡ�ַ���alex_is_good_guy
o."r"  and "rb"������
```
r��ȡһ���ַ�������ɶ�������
rb��ȡһ����������
```
##### p.with������
�Զ�close()
##### q.��withͬʱ�������ļ�
```
with open("file1","r",encoding="utf-8"):
    with open("file2","w",encoding="utf-8"):
        xxxx
```
##### r.����Ĭ�Ϸ���ֵ��
None
##### s.������������ͨ�������ؼ��ֲ������ɱ������Ĭ�ϲ�����ע�����
ʵ�Σ��̶�λ�õĲ��������ǰ���λ��������f(a,b,c,d) ������Ҳ������f(1,2,3,4)���ܶ�һ������һ��
�ؼ��ֲ�����ָ���β���������f(a,b,c,d)  f(a=10,b=20,d=30,c=40) ���ʱ����˳���޹أ����β�û��Ĭ�ϲ����Ϳɱ����ʱ�����ܶ�һ������һ������
�βΣ�Ĭ�ϲ������ڵ���ʱ�ò������Բ�ָ����Ĭ�ϰ���ָ���ã�������Ҳ����ָ��,Ĭ�ϲ������治������ͨ������ɱ����ͨ������f(a,b,d,c=20)
�ɱ�����������������̶������Ǳ�����Ĭ�ϲ���ǰ�棬����ͨ�������棬����Ľ����һ��Ԫ��
����ؼ��ֵĿɱ����������Ľ�����ֵ�

##### t.7.88123���������λС��
```
a=88.45467
print("%0.2f"%a)
```

##### u.�ж���ʽ���Ƿ���ȷ
tpl1="i am {name} age {age} really {name}".format(name="seven",age=18)
tpl2="i am {0} age {1}".format(["seven",18])
```
tpl1��ȷ ��tpl2����ȷ
tpl2="i am {0} age {1}".format(*["seven",18])
```

##### python����ģ��ķ�ʽ��
```
import <ģ����>
import ��.��...<ģ����>
from ��.��.�� import <ģ��>
__import__(ģ��)
```
##### ��ҳ
```
a=192
d,m=divmod(192,10)
if m:
    d+=1
print(d)
```
##### range
```
2.7��range���ص����б�
3.x��range����ֵ�ǵ�����
�б��ǿɵ������ǵ���״̬Ҳ��ֱ�ӿ�����
�������ڵ�����ʱ��Ű������ó���
```
##### sys.argv
```
��������ִ��ʱ�������
xx.py a b
������a��b
```
##### �ϼ�Ŀ¼��·��
../

##### ƴ��·��
a="/sys/seven"
b="lib"
c="bin"
```
import os
print(os.path.join(os.path.join(a,b),c))
```
##### ��ȡ�����ĸ
```
�Ȼ�ȡһ�����������56-90and97-122
��chr()��������
```
##### re��match��search��findall
matchÿһ�д�ͷ��ʼ�ң�����β���û�ҵ�����None���ҵ��󷵻�
##### ������ʽ����group
##### ����1*2+3*4+...
��ʵ��99*100���ٰѽ����97*98�����Դ�����
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

##### json��pickle
##### json��dump��load��dumps��loads����
##### http�����ʷ���ʲô����
##### ��д���������
#####  ð������
```
def sort(li):
	for i in range(1,len(li)):
		for index in range(len(li)-i):
			if li[index]>li[index+1]:
				li[index],li[index+1]=swap(li[index],li[index+1])
```
##### ������Ͷ���

### ������д���
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
����ֵ�Ƿ���ȣ�
```
���
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

֧����������Ĵ���
```
4�У�return inner
5�У�@outer
```
### �����
##### a.��filter,�Զ��庯�����l1�д���33������Ԫ��:l1=[11,22,33,44,55]
```
l1=[11,22,33,44,55]
def f1(x):
    return x>33
de01=filter(f1,l1)
for i in de01:
    print(i)
```
##### b.��filter��lambda��������
```
l1=[11,22,33,44,55]
de01=filter(lambda x:x>33,l1)
for i in de01:
    print(i)
```
##### c.��map���Զ��庯������������Ԫ�ؼ�100:l1=[11,22,33,44,55]
```
l1=[11,22,33,44,55]
def f1(x):
    return x+100 if x%2==1 else x
de02=map(f1,l1)
for i in de02:
    print(i)
```
##### d.��map��lambda����ż����100:l1=[11,22,33,44,55]
```
l1=[11,22,33,44,55]
de02=map(lambda x:x+100 if x%2==0 else x,l1)
for i in de02:
    print(i)
```
##### e.for��ӡ99�˷���
```
def f1(n):
    for i in range(1,n):
        for j in range(1,i+1):
            print("%d*%d=%d"%(j,i,i*j),end="  ")
        print()
f1(10)
```
##### f.�ݹ�ʵ�ֽ׳�
```
def f2(n):
    if n == 1:
        return 1
    return n*f2(n-1)
print(f2(6))
print(1*2*3*4*5*6)
```
##### �����б�
l1=[...]    �����ظ�
l2=[]     ���б�
��С��������

```
import random
l1=[random.randrange(10) for i in range(10)]
l2=[]
'''
˼·��
����l1=[3, 7, 7, 3, 8, 1, 1, 7, 9, 8]
l2=[]

1,�Ȱ���С���ù���l2.append(min(l1))  l2=[1]
2,ѭ��l1�����l1[0]С��l2����������ÿѭ��һ�Σ�ѭ��l2
3,�ж�l1�ĸ����Ƿ�С��l2��ÿһ���������С�ڣ����뵽���±꣬������ڣ��������
4�����l2��ÿһ����С��l1���嵽���
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
#####  װ����
дһ��װ�������ں���ǰ�����before��after
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

### Ӧ����
##### ʹ��requests��xmlʵ��������ѯ
http://www.webxml.com.cn/WebServices/WeatherWebService.asmx/getWeatherbyCityName?theCityName=�Ϻ�

ʵ�֣�
https://github.com/euewrqe/pys16/tree/master/day04/weather_search

