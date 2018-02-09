Shell 的变量功能

变量的取用
```
echo
```


```
name=xjx
echo $name

unset name

name="xjx's parter"
echo $name
```


进入到您目前核心的模块目录
```
cd /lib/modules/`uname -r`/kernel
```


目前我的 shell 环境中， 有多少默认的环境变量,我们可以利用两个命令来查阅，分别是 env 与 export
```
env
```

用 set 观察所有变量 (含环境变量与自定义变量)
```
set
```

PS1：(提示字符的配置)

```
PS1='[\u@\h \w \A #\#]\$ '
```

PID (Process ID)

$字号本身也是个变量,代表的是目前这个 Shell 的线程代号

```
echo $$
```

？问号也是一个特殊的变量,这个变量是`上一个运行的命令所回传的值`, 当我们运行某些命令时， 这些命令都会回传一个运行后的代码。
一般来说，如果成功的运行该命令， 则会回传一个 0 值，如果运行过程发生错误，就会回传错误代码,一般就是以非为 0 的数值来取代

```
echo $?
```

`export： 自定义变量转成环境变量`


子程序仅会继承父程序的环境变量， 子程序不会继承父程序的自定义变量
export：作用分享自己的变量配置给后来呼叫的文件或其他程序


环境变量转成自定义变量, 可以使用 declare

`变量键盘读取、数组与宣告： read, array, declare`

```
[root@www ~]# read [-pt] variable

选项与参数：

-p  ：后面可以接提示字符！

-t  ：后面可以接等待的秒数！

```

```
[root@www ~]# read atest
This is a test
[root@www ~]# echo $atest
This is a test
```

declare 或 typeset 是一样的功能，就是在`宣告变量的类型`。如果使用 declare 后面并没有接任何参数，那么 bash 就会主动的将所有的变量名称与内容通通叫出来，就好像使用 set 一样
```
[root@www ~]# declare [-aixr] variable

选项与参数：

-a  ：将后面名为 variable 的变量定义成为数组 (array) 类型

-i  ：将后面名为 variable 的变量定义成为整数数字 (integer) 类型

-x  ：用法与 export 一样，就是将后面的 variable 变成环境变量；

-r  ：将变量配置成为 readonly 类型，该变量不可被更改内容，也不能 unset

```

范例一：让变量 sum 进行 100+300+50 的加总结果
```
[root@www ~]# sum=100+300+50

[root@www ~]# echo $sum

100+300+50

[root@www ~]# declare -i sum=100+300+50

[root@www ~]# echo $sum

450

```

由于在默认的情况底下， bash 对于变量有几个基本的定义：

1. 变量类型默认为『字符串』，所以若不指定变量类型，则 1+2 为一个『字符串』而不是『计算式』。 所以上述第一个运行的结果才会出现那个情况的；
2. bash 环境中的数值运算，默认最多仅能到达整数形态，所以 1/3 结果是 0；
