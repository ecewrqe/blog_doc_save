[TOC]

## linux文章操作
#### 基礎コマンド
- xargs
build and execute command linesfrom standard input

```
$ cat 1.txt |xargs -L 2
```

- seq 数を生成する
seq [start] [stop] [step]
-s 裂け目、分ける符号, separate numbers

```
$ seq -s : 0, 9
0:1:2:3:4:5:6:7:8:9

# shellのセパレート方法
$ echo {0..10}
0 1 2 3 4 5 6 7 8 9 10
$ echo {0,10,20}
0 10 20
```

- wc: print line, word, byte counts

```
$ cat /etc/passwd|wc
     33      65    1691
# line count  word count  byte count
```
-c: bytes
-m: chars
-l: lines
-w: words

- nl: number lines of files

- tar & zip
压缩tar zcvf xxx.tar.gz /dir/...
解压tar xf xxx.tar.gz

zip, unzip
压缩目录时: zip -r
覆盖: unzip -o
gzip, gunzip

- cut
-d delimiter
-f fields: select only these fields
-b bytes: select only these bytes
-c characters: select only these characters

```
# -
$ echo -e "aaa bbb ccc ddd eee\naaa bbb ccc ddd eee"|cut -d" " -f1-3
aaa bbb ccc
aaa bbb ccc

# ,
$ echo -e "aaa bbb ccc ddd eee\naaa bbb ccc ddd eee"|cut -d" " -f1,3
aaa ccc
aaa ccc
```

- tr: translate or delete characters
tr [set1] [set2]
-d: delete characters in set1

```
$ echo -e "aaa bbb ccc ddd eee"|tr " " ":"
aaa:bbb:ccc:ddd:eee
```

- uniq: report or omit repeated lines
-c: 显示重复的次数在每行之前

```
# echo -e "aaa\naaa\nbbb\nbbb"|uniq
aaa
bbb
```

- sort: sort line of text files
-r reverse
-M: month-sort, -h: human-numeric-sort, -n: numeric-sort, -R random-sort
-t: field-separator=sep
-k: key-sort

- join: join lines of two files on a common field
-a FILENUM
指定不动的文件，另一个文件匹配它
-f FIELD
按照字段合并

```
$ cat a.txt
a
b
c
d
$ cat b.txt
c
f
e
h

$ join a.txt b.txt
a c
b f
c e
d h
```
- paste: merge line of files
-d: delimiters
- tee: >>


#### grep & sed
##### 正規表現
正規表現詳しいドキュメントは[こちら](http://www.kt.rim.or.jp/~kbk/regex/regex.html "こちら")

| 符号 | 説明 |
| --- | --- |
| . | 任意のキャラクター文字をマッチします、改行するものとしないもの |
| * | 演算子直前置かれている部分を可能な限り繰り返しにマッチする、繰り返しの回数が0回でも構わなくてマッチできる、0~n |
| + | \*と同じ直前の演算子を繰り返して、繰り返しの回数少なくでも一回でマッチできる、1~n |
| ? | 直前になる部分正規表現もしくは空文字列をマッチする、0~1 |
| &#124; | 符号の左右両側に置かれた正規表現のいずれかにマッチする |
| [char-list] | 文字のいずれかをマッチする |
| (regexp) | 囲んだ正規表現でマッチした結果をひとまとめに取り扱う、後方参考ができる |
| {n,m},{n,},{n} | 何回繰り返すことが決まってマッチする |
| ^ | 文字列の先頭をマッチする |
| $ | 文字列の最後尾をマッチする |
| [:classname:] | キャラクタークラスを指定する |
| <\> | 単語の区切り, `<group>` |
| `\w` | 数字と文字ずれかにマッチする |
| `\W` | 数字と文字以外のすべてをマッチする |
| `\b` or `\y` | 単語の区切り, `\bgroup\b` |
| `\B` or `\Y` |  |
| `\d` | [0-9] |
| `\D` | [^0-9] |
| `\s` | space |
| `\S` | 非space |
| `\A`, `\Z` | `\A`と^似ている、`\Z`と$似ている、改行してもマッチすることが出来る|
| `(?# text)` | 正規表現中コマンドの付け方 |
| `(?:regexp)` | グループになるけど、後方参考ができない |
| `(?imsx-imsx:pattern)` ||
| `(?P<name>regexp)` | マッチした文字列をnameでつけて、nameで直接探せる |
| `(?{code})` | 埋め込まれたperlコードを実行する |
| `\1,\2, ..., \9` | 丸カッコで囲まれた正規表現でマッチした文字列をインデックスで表す |

各言語に活用している状況

| 符号 | grep extend | sed extend | gawk | perl | python | ruby | tcl | PCRE |
|---|---|---|---|---|
| . | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| * | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| + | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| ? | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| ^ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| $ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| &#124; | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [char-list] | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| (regexp) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| {n,m},{n,},{n} | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [:classname:] | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| <\> | ✔ | ✔ | ✔ | ✖ | ✖ | ✖ | ✖ | ✖ |
| `\1,\2, ..., \9` | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| `\b`,`\B` | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✖ | ✔ |
| `\w`, `\W` | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| `\s`, `\S` | ✖ | ✖ | ✖ | ✔ | ✔ | ✔ | ✔ | ✔ |
| `\d`, `\D` | ✖ | ✖ | ✖ | ✔ | ✔ | ✔ | ✔ | ✔ |
| `\A`, `\Z` | ✖ | ✖ | ✖ | ✔ | ✔ | ✔ | ✔ | ✔ |
| `(?# text)` | ✖ | ✖ | ✖ | ✔ | ✔ | ✔ | ✔ | ✔ |
| `(?:regexp)` | ✖ | ✖ | ✖ | ✔ | ✔ | ✔ | ✔ | ✔ |
| `(?=regexp)` | ✖ | ✖ | ✖ | ✔ | ✔ | ✔ | ✔ | ✔ |
| `(?!regexp)` | ✖ | ✖ | ✖ | ✔ | ✔ | ✔ | ✔ | ✔ |
| `(?imsx-imsx:pattern)` | ✖ | ✖ | ✖ | ✔ | ✔ | ✔ | ✖ | ✔ |
| `(?{code})` | ✖ | ✖ | ✖ | ✔ | ✖ | ✖ | ✖ | ✖ |
| `(?P<name>regexp)` | ✖ | ✖ | ✖ | ✔ | ✔ | ✖ | ✖ | ✔ |

##### grep
grepは三つのパターンモデル: basic, extend, perl
grep, egrep, fgrep: print line matching a pattern
grep [options] pattern [FILE...]

-E: extend_regexp
-P: perl_regexp
-v: invert_match 反してマッチ、排除, -o: only_match
-w: word_regexp, -x: line_regexp
-n: line_number

```
$ stat [file]
  File: ‘a.c’
  Size: 50        	Blocks: 8          IO Block: 4096   regular file
Device: 803h/2051d	Inode: 136483418   Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2019-05-15 13:28:00.852337375 -0400
Modify: 2019-05-15 13:27:55.426620299 -0400
Change: 2019-05-15 13:27:55.426620299 -0400
 Birth: -
```
-A: after-context
-B: before-context
-C: context

character classes:
```
[:alnum:], [:alpha:], [:digit:], [:space:], [:xdigit:]

```

ENVIRONMENT VARIABLES: GREP_COLORS, GREP_OPTIONS

##### sed
stream editor for filter and trasforming text
-f: 使用脚本文件
-r: 扩展正则
-i: 写入
-n: 取消默认输出
```
=: print current line number
a\, i\: append, insert, a\10
r filename: append text from file
=========
d: 删除匹配
p: 打印匹配
g: 全局匹配
s/regexp/replacement/
[n]: n是数字, 第几行; m, n 从m开始取到n行, m~n, 从m行开始每隔n取一行; m,+n 从m行开始取n行,
$: 最后一行
```

#### gawk
pattern scanning and process language
-v var=val: 全局变量
-F fs: 列分隔符, FS
-f program_file: 解析awk脚本文件
-d dump-variables to file
##### 演算子

```
awk '@include "b.awk";{}' /etc/passwd
```

- awk pattern

```
BEGIN
END
/regular expression/

# all the BEGIN or END blocks are merged
|, |&, pipeIO for getline, print, printf
~,!~: regular expression match, negated match, regexp on left of the ~
text ~ regexp

** 乘方
```
control statements
```
@include "file" /pattern/ {statement} /pattern/ {statement}
function name(parameters) {}

if(){}else if(){}else{}
while(){}
do{}while()
for(;;){}
for( k in array ){}
break, continue
delete array[index]
delete array
switch(){
case value|regex:
...
default
}
```

IO statements
```
getline, getline <file: set $0, NF, NR, FNR
getline var, getline var < file, "command" | getline var
next: stop processing the current input record,the next input record read and processing start
nextfile: stop the processing the current file,  go to next file and start processing from first pattern
print: print current record
print expr, print expre >file: print expressions
print ...|command
```

functions
```
gensub(regexp, reptext, mode, source): if not source then from $0
gsub(regexp, reptext, source)
sub(regexp, reptext)
match(s, regexp)
strtonum(str)

strftime(fmt, timestamp)
systime(): return current time string
mktime(time_string): return timestamp
```

- Built-in variables
ARGV: array, file list
ARGC: integer, the number of command line arguments
ARGIND: the index of current file when being processed
FILENAME: current file name
NF: the number of fields in the current input record
NR: the record line of current record
RS,FS: records separater, fields separater

bc: bc is a linux calculate language


#### shell
- profile: when bash is invoke on interactive shell, or `bash --login`
/etc/profile > ~/.bash_profile > ~/.bash_login

- when bash is started from non-interactive shell, read and execute /etc/bashrc

``` shell
((expression))
a=("li" "wang")

if
for name in words;do;done
for ((expr;expr;expr));do;done
select name in words;do;done

while expr;do;done
until expr;do;done
# 左カッコが必要がない
case word in (pattern) list;;...(*) list;;esac
case word in (pattern|parttern) list;;

if expr; then; elif expr; then; else; fi
[[ expr ]] && list || list


for i in ${a[@]};do echo -n $i;done;echo

$* or $@ ⇔ $1 $2 $... # positional parameters, start from $1, IFS is separated parameters
$0 ⇔ scriptname or function name
$? ⇔ exit status
$$ ⇔ process ID of current shell
$! ⇔ process ID of the most recently executed background(asynchronous) command



```

import command: `. <path>`
- command execute

```
$(command), `command`
```

##### parameter expantion参数扩展

```
${parameter}
${parameter:-word}: if parameter is unset or null, the expantion of the word is substituded
${parameter:=word}: if parameter is unset or null, the expantion of the word is assigned to parameter
${parameter:?word}: if parameter is unset or null, the expantion of the word is written to standard error and the shell
${parameter:+word}: if parameter is unset or null, nothing is substtituded, otherwise, the expantion of the word is substtituded
${#parameter}: parameter length

${!name[@]} or ${!name[#]}: name array's key list
${name[@]} or ${name[#]}: name array's value list


${parameter#words} or ${parameter##words}: remove matched prefix words
${parameter%words} or ${parameter%%words}: remove matched suffix words
${parameter/pattern/string}


# slice
${parameter:offset} or ${parameter:offset:length}

```

##### conditional expression状态表达式

```
-a file && -e file: true if file exist
-b file: file exist and a block special file
-c file: file exist and a character special file
-d file: file exist and a dir file
-f file: file exist and a regular file
-g file: is set a group id?
-h file || -L file: is a symbolic link?
-p file: is a named pipe
-r file || -w file || -x file: is readable, writable, executable
file1 -ef file2: True if same device and same inode numbers
file1 -nt file2: true if file1 newer file2, or file1 is exists and file2 is not exists
file1 -ot file2: older
-v varname: True if variable varname is exist
-z string: True if the string length is zero
-n string: True if the string length is non-zero
string1 == string2 or string1 = string2: True if string are equal
string1 != string2: True if not equal

arg: -eq(==), -ne(!=), -lt(<), -le(<=), -gt(>), -ge(>=)


```

##### shell builtin command
- declare & readonly
`typeset`
explicitly declare type to define variable, no maximum limit on the size of an array,give their attribute
```
-a ⇔ indexed array var
-A ⇔ associated array var
-f ⇔ function
-i ⇔ integer, -u ⇔ uppercase, -l ⇔ lowercase
```

unset name
-v variable, -f function

let name=arg
calculate value, $((arg))

- environment

- prompt

```
echo $PS1
\h, \H: hostname

```


- directory
pushd dir, popd

- other

```
coproc $name command
test expr ⇔ [[ expr ]]
test expr1 -a expr2
test expr1 -o expr2
test !expr1

```

#### perl

comment:
```
=pod <name>
...
=cut
<<COMMAND
...
COMMAND
```
input:
``` perl
var=<STDIN>

while(<>){}
/*
1, read every line from file when in command line
2, read every input
*/
```

##### data:

```
my $scalar = any data;
my $sarray = [item1...itemn]
my $hash = {k1=>v1, "k2"=>"v2"}
```

all data in perl is scalar
$: scalar, @: array, %: hash
scalar are always named with $, scalar may be a integer, string, a part of Array or a part of bash

$array[0]: the first element of @array
$hash{"name"}: the 'name' value from hash %hash
$#array: the last index of @array
scalar(@array): the side of array, $#array+1
push(@array, item), pop(@array): append, remove;
shift(@array): return and remove first from @array
unshift(@array, item): add item to last @array
sort(@array): ascii

splice(array, offset, length, )
substr(str, offset, length, replacement)



keys(%hash), values(%hash): hash's key, value @array


@array=(item1, ...,itemn)
%hash=(key1, value1, key2, value2,...)
%hash=(key1=>value1, key2=>value2,...)

scalar has three flavors: number, string, reference

join(separate, @array)->string

``` perl
@colors{'red','blue','green'}
                   = (0xff0000, 0x0000ff, 0x00ff00);
for $k (keys %colors){
    print $k, "\n";
}
qw(a b c) # @array:a b c
```

- general variable

$$: pid
$": list separator
$0: program name;
$(: gid, $<: uid



##### syntax

``` perl
while(expr){BLOCK; next; redo; last}continue{}
until(expr){}continue{}
do{}until()continue{}, do{}while()continue, LABEL{do{last LABEL xxx} until()}continue;
next: continue
last: break
redo: もう一回
goto LABEL;



for var (array) {}
for(expr1; expr2; expr3){}


if(expr){}elsif(){}else{}
unless(expr){}elsif(){}else{}
BLOCK if expr; BLOCK unless expr;
expr && BLOCK, expr and BLOCK
expr?BLOCK1:BLOCK2

# function
sub func_name(parameters){}
```

three builtin data types
scalars, arrays of scalars, associate arrays of scalars

warn "str"

##### regular expression

```
$x = "I like dog";
$x =~ s/dog/cat/; # change the $x value

$y = ($x =~ s/dog/cat/); # $x変えたかどうかと$yに示す
$y=$x =~ s/dog/cat/r; # $x変わらなく$y変えた

$x =~ s/(\w+)/reverse $1/ge # e:execute

$_ = "xxx"  # regexp can defaultly use the $_
# extracting match引き抜きマッチ: $1,$2...
# backreference后向引用: \g1, \g2, ...\gn
```

=~, !~

- first
tr

- last
	$_: spacial default variable
	//=>m//
	//i: ignore case, g: global, r,
	//s: or default, $ can match before newline character,not care newline
	//m: multiple line, ^,$ can match every line first or end between newline character
	//sm: can match long line, but can detect newline, . match any character
	//n: prevent grouping metacharacter() from capture，阻止提取()组抓到的内容，阻止赋值给$1,$2,$...
	s///r: s/// => s{}[]
	s///e: execute perl command
	u: unicode, a: ascii-safe unicode
	l: 按照执行时的编码
	//x: ignore whitespace, can add #comment， 无视空格, 可加#注释
	(?\ms i uald cg): last
	(?\<name\>pattern)=>(?P\<name\>pattern)
	(?:pattern) or (?msiuald\:pattern)
	(?|pattern1|pattern2...): branch reset分支整合
	(?=pattern): positive lookahead assertion积极匹配
	(?!pattern): nagitive lookahead assertion消极匹配
	(?<=pattern): positive lookbehind assertion
	(?<!pattern): nagitive lookbehind assertion
	
	qr(): the variable can embedded to other regexp,
	q(//)=>'', qq()=>"", qx()=>``
	qw(//)=>split(" ", //)
	(?#comment)
tr/// or y///
```
tr/aos/boc/ # a-b, o-o, s-c
tr/aoy/bo/ # a-b, o-o, y-o
```

\X: unicode

``` perl
$x = "There once was a girl\nWho programmed in Perl\n";
$x = <<EOF
There once was a girl
Who programmed in Perl
EOF;
\A is There, \Z is Perl
```

m!! or m{} can be changed to anbitrary words, \s is treated to space character
**space character treated like any other character**

```
/(?<greet>hello)/   # $+{greet}
```

##### subroutine
sub {}->subref
sub name{}
@\_: subroutine parameters array
shift: pop @\_ first item
shift(@arr): pop Array's first item
return

map{$_, xxxx} @arr



##### builtin
defined(var)->bool

my(),local()
delete()
qw(string) -> array

chop, chomp remove last newline  every inputs
split(//, expr)
defined, exist
do(), eval()

my ($index, $item) = each @array
my ($key, $value) = each %hash

grep(expr, any)->0/1
grep{expr}, any->0/1

```
my ($sec,$min,$hour,$mday,$mon,$year,$wday,$yday,$isdst) =
    localtime(time);
```



mkdir, chdir, rename, rmdir


open($fp, "<", inputfile)
open($fp, ">", outputfile)
close($fp)
read($fp, scalar, length, offset)

readline(\<STDIN\>)->$result
readpipe
readlink

print $fp any
``` perl
## read
open($fp, "<", $filename);
while (<$fp>){print $_;}
close(fp);

## write
open($fp, ">", $filename);
print $fp content;
close($fp);

```

ddddd
stat filehandle/ stat dirhandle



- expect
他のアプリと対話する命令
send， send_error, send_log, send_tty, send_user
spawn







