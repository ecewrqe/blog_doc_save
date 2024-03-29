#### 链接
- 集合：http://www.cnblogs.com/wupeiqi/articles/5453708.html
- 其他：http://www.cnblogs.com/wupeiqi/articles/5444685.html
- http://www.cnblogs.com/alex3714/articles/5717620.html
## 序列
### list：

1. 可以随时添加和删除元素
2. 可以做的操作有：切片，添加，删除，查找，修改，排序
3. 列表元素可以是不同类型，在类型不同的情况下就无法排序
4. 列表是有序的

```python
#定义小马列表
li=['twilight','pinkiepie','rainbow dash']
```
添加:appand(self->list,x->all)->None
```
li.append('apple jack')
print("after append:%s"%li)
```
插入:insert(self->list,i->int,x->all)->None
```
li.insert(1,'rarity')
print("after insert:%s"%li)
```
##### 删除
remove(self->list,x->all)->None
```
li.remove('twilight')
print("after remove:%s"%li)
```
pop(self->list,i->int(default:-1))->all   默认删除最后一个
```
li.pop(1)
print("after pop:%s"%li)
```
del
```
del li[2]   #表示删除li中的某一个值
del li    #整个li都删除
```
排序:sort(self->list,cmp->\[all,all\](default:None),key->\[all\](default:None),reverse->bool(default:False))->None
```
li.sort()
print("after sort:%s"%li)
```
倒序:reverse(self->list)->None
```
li.reverse()
print("after reverse:%s"%li)
```
合并两个列表:extend(self->list,t->iterator)->None   -----t可以是任何迭代器
```
li.extend(['twilight','rainbow dash'])
print("after extend:%s"%li)
```

计算元素的个数:count(self->list,x->all)->int  返回元素的个数，-1为没有找到
```
print("rainbow dash个数:%s"%li.count('rainbow dash'))
print("li 元素个数:%s"%len(li))
```
查找元素的下标:index(self->list,x->all,start->int,stop->int)->int    如果找不到就报错
```
print("第二个rainbow dash在哪里？%s"%li.index('rainbow dash',2))
#遍历列表,sys.stdout.write为了不换行输出
import sys
for i in li:            #li.__iter__()是list的迭代器,放在这里效果一样
    sys.stdout.write(i+",")
print()
```


### tuple
元祖是不可修改的列表，同时元祖也被看作一条记录
```
ipattr=("127.0.0.1",9999)
#只有两个方法：index和count
# 把元祖看作记录
city,year,month,day,hour,min=('Beijing',2017,7,23,11,41)
#占位符_
city,year,_,day,hour,min=('Beijing',2017,7,23,11,41)
*_,month,day,*_=('Beijing',2017,7,23,11,41)
#嵌套元祖
city,date,time=('Beijing',(2017,7,23),(11,41))  # 不往下一层深入，就这样，也可以往下一层深入
city,(year,month,day),(hour,min)=('Beijing',(2017,7,23),(11,41))

'''
*可以把可迭代对象拆开，目的：
1，作为参数传递给函数
2，在赋值的时候作为剩余参数来处理(python3)
*赋给的值是列表
'''
a,b,*rest=range(5)   #*是有多少填充多少，直到填充满
rest表示[3,4,5]
a,b,*rest,e=range(5)
rest表示[3,4]

# 具名元祖
City=namedtuple('City','city year month day hour min')  #底层创建的是一个类
beijing=City('Beijing',2017,7,23,11,41)
print(beijing)
print("item:",beijing.year)
print("cut:",beijing[1:])

```
### 切片
```
>>> s= 'bicycle'
>>> s[2:5:1]
'cyc'
>>> s[2:]
'cycle'
#切片实质是调用slice(start,stop,step)
#start和stop为负，从最后计数，step为负，从最后开始往前：如[::-1]倒叙拍
>>> s[slice(0,-1)]
```
### 序列+和*运算
```
l1=[1,2,3]
l2=[4,5,6]

l3=l1+l2
l4=l1*3
# 序列相加和相乘不会改变元素的id，会组成一个新的列表，列表里的每个元素都是相加之前的。


```


## dict
字典是以键值对来表示，键是唯一的，可以是数字或者字符串，值可以是任意重复的。字典在底层是hash，散列表。
就把hash想象成字典，字典的目录就是一张hash表，不可变，一个数对应一个键，这里的数是有序的数，拿到数就能找到值，因为它们是关联的。而如何找到键，只要如何找到这个有序的数中的对应值，就能拿到数据。==字典是无序的==

字典的操作：取值，键值分离，键值查找，添加，删除等
```
#定义小马字典
ponys={'ts':'twilight','pp':'pinkiepie','ponyChild':["little apple","little rary","little RD"]}
```
#### 查找:
```
ponys['ts']   #找到twilight
ponys['ponyChild'][0]    #找到little apple
ponys['ponyChild'][2]="little rainbow dash"
```
get(self->dict,key->(str/int/float),default->all)->all   查找的内容，返回值为找到的内容，没有找到返回None，default有定义，则返回返回default的内容
```
print(ponys.get('ts'))    # twilight
print(ponys.get('rd'))    # None
print(ponys.get('rd',"rainbow dash"))     # rainbow dash
```
设置：setdefault(self->dict,key->key,default->all)->all    在dict中查找key，找到后返回value，如果没找到，自动创建一个，
并返回None；值默认设为None,default为没有找到的情况下创建的键的值，
```
ponys.setdefault("ts","rainbow dash")
print(ponys)
```

### 生成器表达式
```
#如果我有这么一串字符，需要找到unicode值>20000的汉字的值
symbols='拿不动'
beyoud_ascii=[ord(s) for s in symbols if ord(s) > 20000]
print(beyoud_ascii)

```
```
#如果Tshirt的样式有这么几种，生成笛卡尔积
colors=["black","white"]
sizes=['S','M','L']

#拿出颜色为black的所有Tshirt
[(color,size) for color in colors if color == 'black' for size in sizes]
#中括号内部的这块内容就是生成器表达式，它默认不会把6个都列出来。只会一个个生成
拿出大小为S的所有Tshirt
[(color,size)for color in colors for size in sizes if size == 'S']

以生成器的方式表示
((color,size)for color in colors for size in sizes)
```


#### 迭代：
```
for key in ponys:
    print(key)
#迭代的结果为所有key，取值看下面
for key in ponys:
    print(ponys[key])
```
提取所有键:keys(self->dict)->iterable(dict_keys)   ==返回dict_keys:dict_keys不是列表但可以迭代，必须用list强制转换成列表，才能使用列表的功能==

提取所有值:values(self->dict)->iterable(dict_values)   同上

提取所有的键值，形成元祖列表:items(self->dict)->iterable(dict_items)   每一组键和值形成元祖，所有的键值元祖形成列表，
但是该列表不是真正的列表，需要强制转换
```
print(ponys.keys())
print(ponys.values())
print(ponys.items())
#内容
#dict_keys(['ponyChild', 'ts', 'pp'])
#dict_values([['little apple', 'little rary', 'little RD'], 'twilight', 'pinkiepie'])
#dict_items([('ponyChild', ['little apple', 'little rary', 'little RD']), ('ts', 'twilight'), ('pp', 'pinkiepie')])
```

#### 删除
pop(self->dict,key->key,default->all)->all   删除指定key，返回value，
如果没找到key，default如果有内容会返回，否则会报错

#### 更新
update(self->dict,other->all,*arg) ->None

```
#ponys.update({"rd":"rainbow dash","aj":"apple jack"})
ponys.update(rd="rainbow dash",aj="apple jack")
print(ponys)
```

#### other

fromkey(self->dict,seq->iterator,value->all)->dict
```python
ponys={}
ponys_name=["ts","pp"]
ponys=ponys.fromkeys(ponys_name,3)    #3这个值是被ponys_name中的所有key引用
print(ponys)
# 得到
# {'pp': 3, 'ts': 3}
```

## 集合
```python
set1={"pp","rr","tw"}
set2={"rd","pp","tw"}
print(set1,set2)
```
#### 添加删除元素
```python
set1.add("aj")
set1.remove("tw")

#通用的输出函数
def output(*arg):
    fm1="{0}是{1}和{2}的{3}"
    fm2="{0}{1}{2}的{3}"
    if arg[1]==True:
        print(fm2.format(arg[0],"是",arg[2],arg[3]))
    elif arg[1]==False:
        print(fm2.format(arg[0],"不是",arg[2],arg[3]))
    else:
        print(fm1.format(arg[0],arg[1],arg[2],arg[3]))
```
#### 并集  |
```python
set3=set1.union(set2)     #set3=set1|set2   set1|=set2
output(set3,set1,set2,"并集")
# 得到：
{'tw', 'rr', 'pp', 'rd'}是{'tw', 'rr', 'pp'}和{'tw', 'rd', 'pp'}的并集
```
#### 交集 &
```python
set3=set1.intersection(set2)        #set3=set1&set2   intersection_update更新对象，没有返回值
output(set3,set1,set2,"交集")
# 得到：
{'tw', 'pp'}是{'tw', 'rr', 'pp'}和{'tw', 'rd', 'pp'}的交集
```
#### 差集 -
```python
set3=set1.difference(set2)          #set1=set1-set2     difference_update
output(set3,set1,set2,"差集")
set3=set2.difference(set1)         #set1=set2-set1
output(set3,set2,set1,"差集")
# 得到：
{'rr'}是{'rr', 'tw', 'pp'}和{'tw', 'rd', 'pp'}的差集
{'rd'}是{'tw', 'rd', 'pp'}和{'rr', 'tw', 'pp'}的差集
```
#### 对称差集 ^
```python
set3=set1.symmetric_difference(set2)   #set1=set1-set2|set2-set1     symmetric_difference_update
output(set3,set1,set2,"差集二")    #set1=set1^set2
# 得出：
{'rr', 'rd'}是{'rr', 'pp', 'tw'}和{'rd', 'pp', 'tw'}的对称差集
```

#### 子集
```python
set3={"pp","rr"}
output(set3,set3.issubset(set1),set1,"子集")
output(set1,set1.issubset(set3),set3,"子集")
# 得出：
set3={"pp","rr"}
output(set3,set3.issubset(set1),set1,"子集")
output(set1,set1.issubset(set3),set3,"子集")
```

#### 父集
```python
set3={"pp","rr"}
output(set3,set3.issuperset(set1),set1,"父集")
output(set1,set1.issuperset(set3),set3,"父集")
# 得出：
{'pp', 'rr'}不是{'tw', 'pp', 'rr'}的父集
{'tw', 'pp', 'rr'}是{'pp', 'rr'}的父集
```

### collection符合类型

### array数组
```
import array
arr=array.array('I',[1,2,3])
print(arr[0])
```
数组是有类型的，一个数组只能存放一个类型的元素，在其他语言中数组是不可变长度，同种类型的一长串序列，在内存中顺序存储
但是python的array可以append，可以像list一样使用特性。


