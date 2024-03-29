##### 代码上线后----开放封闭原则
1. 一定不能修改源代码
2. 不能修改函数调用方式
3. 而同时又要能增加功能
装饰器用来解决上述问题，装饰器实质是一个函数
#####装饰器调用:
``` python
@wrapper01   # 此处为装饰器的调用，做了这件事：f1=wrapper01(f1)
def f1(arg):
	print(arg)

f1("hello")  #没加装饰器之前调用的是f1，
# 加了装饰器后调用的是wrapper01函数的返回值()，加()得是函数，wrapper01就应该返回一个函数

#来一招釜底抽薪
def f2(arg):
    print("f2",arg)
def wrapper01(func):
    return f2

@wrapper01
def f1(arg):
    print(arg)

f1("hello")
返回结果是f2 hello
# 装饰器实现
def wrapper01(func):
    def inner(arg):
        func(arg)
    return inner

#完整装饰器
def wrapper01(func):
    def inner(*args,**kwargs):
        #加内容
        temp=func(*args,**kwargs)
        #加内容
        return temp
    return inner
```
### 装饰器叠加
```
@wrapper02   #f1=wrapper02(f1)  -> f1=wrapper02(wrapper01(f1))
@wrapper01    #f1=wrapper01(f1)
def f1(arg):
    print(arg)

f1("hello")
```

### 有参装饰器
``` python
@wrapper01(p1,p2)  #@后面的内容是wrapper01(p1,p2)的返回值
def f1(arg):
    print(arg)

f1("hello")
```
##### 实现
```
def wrapper01(p1,p2):
    print(p1,p2)
    def wrapper01_ret(func):
        def inner(*args,**kwargs):
            #加内容
            temp=func(*args,**kwargs)
            #加内容
            return temp
        return inner
    return wrapper01_ret

@wrapper01("k1","v1")   # @wrapper01("k1","v1")==@wrapper01_ret
def f1(arg):
    print(arg)

f1("hello")
```