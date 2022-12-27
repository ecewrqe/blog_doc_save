##### re�����ʽ
| ����        | ˵��           | 
| ------------- |:-------------:|
| ^      | ƥ�俪ͷ | 
| $      | ƥ���β     | 
| [...] | ƥ������һ��ָ�����ַ�   |
| [^...] | ƥ������һ����ָ�����ַ�   |
| * | ƥ��0�������ַ�ǰ����ַ���û��Ҳ��   |
| + | ƥ������һ������ǰ����ַ�   |
| {n} | ƥ��n��ǰ����ַ�,��1��ʼ   |
| {,n} | ƥ�����n��ǰ����ַ�   |
| {m,} | ƥ������m��ǰ����ַ�  |
| {m,n} | ƥ������m���n��ǰ����ַ�   |
| a|b | ƥ��a��b   |
| \w | ƥ������һ���������ַ�   |
| \W | ƥ������һ�������ַ�   |
| \d | ƥ������һ������   |
| \D | ƥ������һ��������   |
| (...) |  	ƥ����   |
| (?P<name\>...) | ƥ��������ֵ�   |
| (?#...) | ע��   |

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
| ����        | ˵��           | 
| ------------- |:-------------:|
|re.I      | ��ʶ���Сд | 
|re.M      | ����ģʽ | 

##### ƥ�䷽��
match(self->re,pattern,string,flags)->re_match_obj
search(self->re,pattern,string,flags)->re_match_obj
findall(self->re,pattern,string,flags)->list
split(self->re,pattern,string,flags)->list
sub(self->re,pattern,repl,string,count,flags)->str

##### sub��������
``` python
re.sub(r"(.*)  (.*)",r"\1 \2",msg)
```


##### match����ķ���
group(self->match,\*args(0,1,2...)) ->str/tuple  # ��Ŵ�1��ʼ
groups(self->match,default)->tuple             # ����Ԫ��
groupdict(self->match,default)->dict          # (?P\<name\>...)��ʽ�ķ��ؼ�ֵ

span(self->match,\*args(0,1,2...))->tuple           # ƥ�������(0,17)


