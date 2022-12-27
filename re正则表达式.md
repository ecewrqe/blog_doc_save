##### re正则格式
| 符号        | 说明           | 
| ------------- |:-------------:|
| ^      | 匹配开头 | 
| $      | 匹配结尾     | 
| [...] | 匹配任意一个指定的字符   |
| [^...] | 匹配任意一个非指定的字符   |
| * | 匹配0个或多个字符前面的字符，没有也算   |
| + | 匹配至少一个或多个前面的字符   |
| {n} | 匹配n个前面的字符,从1开始   |
| {,n} | 匹配最多n个前面的字符   |
| {m,} | 匹配至少m个前面的字符  |
| {m,n} | 匹配至少m最多n个前面的字符   |
| a|b | 匹配a或b   |
| \w | 匹配任意一个非特殊字符   |
| \W | 匹配任意一个特殊字符   |
| \d | 匹配任意一个数字   |
| \D | 匹配任意一个非数字   |
| (...) |  	匹配组   |
| (?P<name\>...) | 匹配组组成字典   |
| (?#...) | 注释   |

``` python
import re
strdd="eue123@www456.org|ddd"
temp=re.match("([a-zA-Z0-9_]+)@(?P<comp>\w+).(com|org)",strdd)
print(temp.group(3))
temp=re.match("(?P<name>\w+)@(?P<comp>\w+).(?P<last>\w+)",strdd)
print(temp.group())
print(temp.groupdict())
```

##### flags
| 符号        | 说明           | 
| ------------- |:-------------:|
|re.I      | 不识别大小写 | 
|re.M      | 多行模式 | 

##### 匹配方法
match(self->re,pattern,string,flags)->re_match_obj
search(self->re,pattern,string,flags)->re_match_obj
findall(self->re,pattern,string,flags)->list
split(self->re,pattern,string,flags)->list
sub(self->re,pattern,repl,string,count,flags)->str

##### sub后向引用
``` python
re.sub(r"(.*)  (.*)",r"\1 \2",msg)
```


##### match对象的方法
group(self->match,\*args(0,1,2...)) ->str/tuple  # 组号从1开始
groups(self->match,default)->tuple             # 返回元组
groupdict(self->match,default)->dict          # (?P\<name\>...)格式的返回键值

span(self->match,\*args(0,1,2...))->tuple           # 匹配的数量(0,17)


