##### �������ߺ�----���ŷ��ԭ��
1. һ�������޸�Դ����
2. �����޸ĺ������÷�ʽ
3. ��ͬʱ��Ҫ�����ӹ���
װ������������������⣬װ����ʵ����һ������
#####װ��������:
``` python
@wrapper01   # �˴�Ϊװ�����ĵ��ã���������£�f1=wrapper01(f1)
def f1(arg):
	print(arg)

f1("hello")  #û��װ����֮ǰ���õ���f1��
# ����װ��������õ���wrapper01�����ķ���ֵ()����()���Ǻ�����wrapper01��Ӧ�÷���һ������

#��һ�и��׳�н
def f2(arg):
    print("f2",arg)
def wrapper01(func):
    return f2

@wrapper01
def f1(arg):
    print(arg)

f1("hello")
���ؽ����f2 hello
# װ����ʵ��
def wrapper01(func):
    def inner(arg):
        func(arg)
    return inner

#����װ����
def wrapper01(func):
    def inner(*args,**kwargs):
        #������
        temp=func(*args,**kwargs)
        #������
        return temp
    return inner
```
### װ��������
```
@wrapper02   #f1=wrapper02(f1)  -> f1=wrapper02(wrapper01(f1))
@wrapper01    #f1=wrapper01(f1)
def f1(arg):
    print(arg)

f1("hello")
```

### �в�װ����
``` python
@wrapper01(p1,p2)  #@�����������wrapper01(p1,p2)�ķ���ֵ
def f1(arg):
    print(arg)

f1("hello")
```
##### ʵ��
```
def wrapper01(p1,p2):
    print(p1,p2)
    def wrapper01_ret(func):
        def inner(*args,**kwargs):
            #������
            temp=func(*args,**kwargs)
            #������
            return temp
        return inner
    return wrapper01_ret

@wrapper01("k1","v1")   # @wrapper01("k1","v1")==@wrapper01_ret
def f1(arg):
    print(arg)

f1("hello")
```