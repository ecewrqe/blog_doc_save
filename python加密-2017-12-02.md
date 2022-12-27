### (md5,sha)加密
md5和sha家族都是不可逆加密 ,用来识别文件或图片的唯一性
```
import hashlib
m = hashlib.md5()
m.update(b'ddd')
print(m.hexdigest())
# 其他sha1/sha256格式类似
```

### base64
base64是基于64个可打印字符(alpha)来表示二进制的数据
```
import base64
base64.encodestring(b'ddd')
base64.b64encode(b'ddd')
```
二进制下每8位对应成一个字符，所以是可逆的加密

### binascii
