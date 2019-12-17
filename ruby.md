
BEGIN, END

スカラー変数
$0 filename
$1...$n: the previous regexp group

Regexp =~ string
Regexp.match(string)
string.match(Regexp)


hash: {:key=>value, } or {key: value, key: value}
h1[:key]

chr ⇔ ord

### control expression
``` ruby
# if and unless
if expr/regexp (then)
elsif expr (then)
else
end
IFLIST if expr

# ternary if 3値演算子
a>b?a:b

# case expresson
case va
when expr/regexp then
CASELIST
else
CASELIST
end
‘
# while and until
while expr (do)
end
WHILELIST while expr

begin
end while expr

for var in arr
end

break, next, redo # perl: last, next, redo

begin
rescue => exception
else

end

func(parameters){|x| BLOCK }
yield(*args) send some variable to block


# lambda

twice = ->(x){x*2} # twice.call(2)
twice = lambda{|x| x*2}  # twice.call(2)
```

### block
given_block?: was received block
```
def func
	if given_block?
		yield(x, y, ...)
	else
	end
end
```

global value: $var

- class

class value: @@var
local value: @var
constructor: def initialize

attr_reader
attr_writer
attr_accessor: read and write

function : !赋值, ?返回bool













