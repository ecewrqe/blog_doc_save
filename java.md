
[java document](https://docs.oracle.com/en/java/javase/13/ "java document")


### 类型转换
char, byte, short->int->long->float->double
```
char -128~127 1字符
byte -128~127 1字节
short  Int16
int 4字节 Int32， long 8字节 Int64
float，double

1, 容量小的类型转换成容量大的类型，自动转型
2，容量大的转换成容量小的要强制类型转换， 要考虑溢出
3, 默认整数是int, 默认实数是double
4, float实数要标识f或者(float), long整数要标识long或者(long)
5，char, byte, short不做运算，一旦做运算，自动转换成int类型
6, 布尔类型不能转换成其他任何类型

csharp有sbyte，是一个无符号字节类型
```

### 多态 polymorphism
向上转型  子-->父, 自动类型转换
向下转型  父-->子, 强制类型转换

overload方法重载
相同的方法名，不同的参数数量，不同的参数顺序，不同的参数类型，不同的参数名称， 不同的返回值 互为重载的两个方法不能一样
重载也是多态的一种， 不同的子类可以拥有不同的该方法的不同形态， 都可以由父类去调用

override重写，实现，覆盖：
两个具有继承关系的类
有相同的方法名, 相同的返回值类型， 有相同的参数名和参数数量
继承最基本的作用: 代码重用， 最重要的作用: 重写
重写的方法不能比被重写的方法拥有更低的访问权限
重写方法不能比被重写的方法抛出更宽泛的异常
构造方法不能被继承
静态方法不存在覆盖
重写指的是成员方法，和成员变量无关

- super

```
# java
super(), super.xxx()
# csharp
base(), base.xxx()
public void m01(): base(){}
```

### 抽象  abstract
1, 抽象方法由abstract修饰
2, 有抽象方法的类称为抽象类，抽象类必须加abstract修饰
3, 不能使用final修饰抽象类的成员变量
4, 抽象类不能直接使用， 必须要继承

接口是纯抽象， 完全抽象，接口没有构造方法
```
java: final class, CSharp: sealed class 同じ効果、更に継承を制限するために

java: final int a=> const int a
CSharp: readonly int a=> const int a

java: public abstract void f01(){}
CSharp: public virtual void f01(){}
```



### Collection
java

```
import java.util.*

List: ArrayList<T>(), LinkedList<T>(), Vector<T>
Set: HashSet<T>, TreeSet<T>
Map: HashMap<T>, TreeMap<T>

queue: ArrayDeque
stack: Stack
```

CSharp

```
using System.Collections.generic

List: List<T>(), LinkedList<T>()
Set: HashSet<T>(), SortedSet<T>()
Dict: Dictionary<K, V>(), SortedDictionary<K, V>()
queue: Queue
stack: Stack
```

### util.Array



### date
java
```
import java.util.Date
Date d01 = new Date()

import java.text.SimpleDateFormat
DateFormate df = SimpleDateFormat(fmt) # fmt yyyy-MM-dd HH:mm:ss SSS
df.format(d01)

df.parse("xxxx-xx")->Date throw ParseException
```

Csharp
```
System.DateTime

var d01 = new DateTime(year, month, day, ...)
var d01 = DateTime.Now
d01.addDay,...
DateTime.ParseExact(s, format, System.Globalization.CultureInfo.InvariantCulture)->Date
d01.toString("yyyyMMdd")

```

### file read and write


### path

java.io.File
java.io.FileFilter
java.io.FileNameFilter

```
current path
new File(".")->file
file.getAbsolutePath() # f:/xxx/xxx/xxx/.
file.getCanonicalPath() # canonical 簡潔, f:/xxx/xxx/xxx/
getName, getParent, getPath

file.isAbsolute(), isFile(), isPath(),

file.canRead(),canWrite(),canExecute
file.setReadable(boolean), file.setWriteable(boolean), file.setExecutable(boolean)

file.deleteOnExit()  # 当java虚拟机停止之前删除文件

file.mkdir()->boolean  # 创建当前目录
file.mkdirs()->boolean  # 全局创建目录，包括父级任何没有被创建的目录

file.renameTo(String dest)->boolean
```




### encoding
java.nio.charset.StandardCharsets


string->byte
str.getBytes(charset)->byte[]
real_str = new String(bytes, charset)

### Iterator

```
Collection c01 = new Vector();
c01.add(1);
c01.add(2);
c01.add(3);

Iterator i01 = c01.iterator();
while(i01.hasNext()){
i01.next();

}
```



二つオブジェクトの内容同じの判定にはhashCode(), equals(Object o)ではhash値同じながらequals返した値はtrue

containsKey(String s)->boolean




