一个持续input的程序
```
while True:
   text=input("性暗示:")
   print(text)
----
性暗示:Traceback (most recent call last):
  File ".\jumpserver_shell.py", line 19, in <module>
EOFError
每次ctrl+c都是EOFError或者KeyboardInterrupt
------
while True:
    pass
或者
time.sleep(20)
中ctrl+c
只会显示KeyboardInterrupt
```

把这个异常捕获了如何
```
while True:
   try:
       print("ddddd")
       except KeyboardInterrupt as e:
           pass
----
ctrl+c就没有什么问题，会一直打印ddddd
```
在input的时候捕获,似乎 第一次时EOFError第二次是Keyboard
```
while True:
    try:
        try:
            input()
        except EOFError as e:
            print("还没结束")
    except KeyboardInterrupt as e:
        print("keyboard还没结束")
-----
只有第一次是"还没结束"
其余都是"keyboard还没结束"
```
以上这种方法只能在输入ctrl+c的时候，并且捕获后处理的时间不是很长，才有效。
尝试在输入的时候，在任务管理器停掉这个python程序，也一样无效

### linux的signal信号处理
python的signal模块
```
def signal_handle(signum,other):
    global signal_flag
    print("鸡儿想要放假？")
    time.sleep(1)

import signal
signal.signal(signal.SIGINT,signal_handle)
while True:
    text = input("pls input:")
----
SIGINT接受ctrl+c的信号
SIGTERM接受普通kill的信号，有些信号没法捕获ctrl+d，kill 9没法捕获，直接被杀死
```
以上这种可以在 linux中解决大多数问题，尝试用shell的signal机制

trap <命令>  信号们

信号们
名称    值     描述
SIGHUP  1      控制终端发现被挂起或控制进程死亡
SIGINT  2      键盘终端
SIGQUIT 3      来自键盘的退出信号
SIGKILL 9      杀死进程的信号
SIGALRM 14     定时时钟中断
SIGTERM 15     终止信号

```bash
trap "" 1 2 3 9 15

while [ true ]
do
python3 s1.py   # 内部负责捕获异常，外部负责捕获信号
echo "eof"
done
```

trap负责捕获，捕获到了之后干什么，是引号中的内容，没有内容表示无视。
shell有完整的信号处理机制，但是kill -9就完蛋

在终端，如何让密码不显示：
```
import termios
#保存原来 终端的状态
oldtty=termios.tcgetattr(sys.stdin)

#回到原来终端的状态
termios.tcsetattr(sys.stdin,termios.TCSADRAIN,oldtty)
```









