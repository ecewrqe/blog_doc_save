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

string =~ regexp

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