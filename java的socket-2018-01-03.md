socket传输是IO流传输。所以和io有关：
socket绑定ip和端口后accept等待客户端socket。拿到socket后要进行流操作。socket传过来是字节流，可以转成字符流。
InputStreamReader()传入字节流，将字节流转成字符流。

服务器永远是先被动。客户端主动连接，要主动断开。如果客户端没有close()就结束程序，服务端会报错：
```
java.net.SocketException: Connection reset
```

