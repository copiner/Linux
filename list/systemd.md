### 系统的启动控制

RHEL5、RHEL6、RHEL7的init系统分别为sysvinit、upstart、systemd


linux启动过程

linux运行级别：runlevel


Linux 服务管理两种方式service和systemctl

service命令

service命令其实是去/etc/init.d目录下，去执行相关程序
```shell
# service命令启动redis脚本
service redis start
# 直接启动redis脚本
/etc/init.d/redis start
# 开机自启动
update-rc.d redis defaults
```

RHEL7 服务控制单元systemd,`6.x 不支持systemd。只能使用service命令进行服务管理`

`/etc/lib/systemed/system`

```shell
# 查看Linux内核版本命令
cat /proc/version
#查看Linux系统版本的命令
cat /etc/issue

# 查看系统单元
systemctl
systemctl list-units
# 查看运行失败的单元
systemctl --failed

#查看系统中安装的服务
systemctl list-unit-files
```

单元名结尾的扩展名标识了单元的类型，对应关系如下

```
.device : 能被内核识别的设备
.service : 系统中的服务
.timer : 定时器
.socket : 进程间通讯套接字
.snapshot : 系统服务状态管理
...
```

```shell
#启动服务单元
systemctl start xxx
service xxx start

# 停止服务单元
systemctl stop xxx
service xxx stop

#查看单元运行状态
systemctl status xxx
service xxx status

#重新读取配置
systemctl reload xxx

# 查询服务是否位自动启动
systemctl is-enabled xxx

#将服务设置为自动启动
systemctl enable xxx

# 取消服务自动启动
systemctl disable xxx

# 关机
systemctl poweroff
#重启
systemctl reboot
#待机
systemctl suspend
```

### Linux 进程管理

进程ID : PID

父进程ID : PPID

用户ID : UID

所属组ID : GID

进程状态：R(run)、S(sleep)、Z(zombie)

进程管理工具和常用命令: ps、top、tree;
```shell
# ps常用参数
ps -aux | head

ps -ef | grep nginx

```

ps结果项说明
```shell
USER : 表示启动进程的用户
PID : 进程的序号
%CPU : 进程占用的CPU百分比
%MEM : 进程使用的物理内存百分比
VSZ : 进程使用的虚拟内存总总量，单位KB
RSS : 进程使用的、未被换出的物理内存大小，单位KB
TTY : 终端ID
STAT : 进程状态
START : 启动进程时间
TIME : 进程消耗CPU的时间
COMMAND : 启动命令的名称和参数
```

STAT进程状态结果说明
```shell
D : 不能被中断的进程
R : 运行中
S : 处于休眠状态
T : 停止或被追踪
W : 进入内存交换（从内核2.6开始无效）
X : 死掉的进程
Z : 僵尸进程
< : 优先级较高的进程
N : 优先级较低的进程
s : 该进程有子进程
L : 多线程宿主
+ : 位于后台的进程组

PROCESS STATE CODES
Here are the different values that the s, stat and state output specifiers
(header "STAT" or "S") will display to describe the state of a process.

D Uninterruptible sleep (usually IO)
R Running or runnable (on run queue)
S Interruptible sleep (waiting for an event to complete)
T Stopped, either by a job control signal or because it is being traced.
W paging (not valid since the 2.6.xx kernel)
X dead (should never be seen)
Z Defunct ("zombie") process, terminated but not reaped by its parent.

For BSD formats and when the stat keyword is used, additional characters may
be displayed:
< high-priority (not nice to other users)
N low-priority (nice to other users)
L has pages locked into memory (for real-time and custom IO)
s is a session leader
l is multi-threaded (using CLONE_THREAD, like NPTL pthreads do)
+ is in the foreground process group
```

top命令监视系统的实时状态 top
```shell
top - 16:06:14 up 265 days,  3:26,  3 users,  load average: 0.00, 0.00, 0.01
Tasks:  96 total,   1 running,  95 sleeping,   0 stopped,   0 zombie
Cpu(s):  3.4%us,  0.1%sy,  0.0%ni, 96.3%id,  0.2%wa,  0.0%hi,  0.0%si,  0.0%st
Mem:   8061624k total,  7473684k used,   587940k free,   245944k buffers
Swap:  4063224k total,        0k used,  4063224k free,  3347760k cached

PID USER    PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND    
1 root      20   0 19356 1528 1224 S  0.0  0.0   0:04.39  init      
2 root      20   0     0    0    0 S  0.0  0.0   0:00.97 kthreadd   
3 root      RT   0     0    0    0 S  0.0  0.0   0:02.20 migration/0
4 root      20   0     0    0    0 S  0.0  0.0   0:28.28 ksoftirqd/0
5 root      RT   0     0    0    0 S  0.0  0.0   0:00.00 migration/0
6 root      RT   0     0    0    0 S  0.0  0.0   0:26.41 watchdog/0
7 root      RT   0     0    0    0 S  0.0  0.0   0:02.21 migration/1
```
上例信息解释如下：

当前系统时间16:06:14,系统启动了265天，3小时26分钟，目前登陆系统中的用户有3个，load average后面3个值
最近1分钟、5分钟、15分钟的系统负载量。此部分值可参考CPU的个数，如果超过CPU个数的两倍以上，说明系统
高负载，需立即处理，小于CPU的个数表示系统负载不高，服务器处于正常状态。

Task部分：有96个进程在内存中，其中1个正在运行，95个在睡眠，0个处于停止状态，0个处于僵尸状态。

Cpu部分:
```shell
%us(usr) : 用在用户态程序上的时间
%sy(sys) : 用在内核态程序上的时间
%ni(nice) : 用在nice优先级调整过的用户态程序上的时间
%id(idle) : CPU空闲时间
%wa(iowait) : CPU等待系统IO的时间
%hi : CPU处理硬件中断的时间
%si : CPU处理软件终端的时间
%st(steal) : 用于有虚拟CPU的情况，用来指示被虚拟机偷掉的CPU时间。
```
通常idle值反映一个系统CPU的闲忙程度。另外，如果用户态进程user的CPU百分比持续为95%以上，说明应用程序需要优化。

Mem部分：总内存、已使用内存、空闲的内存、用于缓存文件系统的内存。实际占用内存：(8061624-587940-245944)kb

Swap部分：交换空间总大小、使用的交换内存空间、空闲的交换空间、用于缓存内容的交换空间。

默认情况下，top命令每隔5秒刷新一次数据。top命令常用参数列表如下：
```shell
-b : 以批量模式运行，但不能接受命令行输入
-c : 显示命令完整启动方式，而不仅仅是命令名
-d N : 设置两次刷新之间的时间间隔
-i : 禁止显示空闲进程或僵尸进程
-n : 显示更新次数，然后退出
-p PID : 仅监视指定进程ID
-u : 只显示指定用户的进程信息
-s : 安全模式运行，禁用一些交互指令
-S : 累积模式，输出每个进程的总的CPU时间，包括已死的子进程。
```

top在运行时可以接收一定的命令参数,如下：
```shell
blankspace : 立即更新当前状态
c : 显示整个命令，包含启动参数
f,F : 增加显示字段，或删除显示字段
m : 切换到内存信息，并以内存占用大小排序
n : 输入后将显示指定数量的进程数量
t : 切换到显示进程和CPU状态的信息
A : 按进程生命大小进行排序，最新进程显示在最前面
H : 显示有关安全模式及积累模式的帮助信息
M : 按内存占用大小排序，由大到小
N : 以进程ID大小排序，由大到小
P : 按CPU占用情况排序，由大到小
q : 退出top程序
```


进程的启动

Linux的进程分为前台进程和后台进程，前台进程会占用终端窗口，而后台进程不会占用终端窗口。
要启动一个前台进程，只需要在命令行输入启动进程的命令即可，要让一个程序在后台运行，
只需要在启动进程时，在命令后加上`&`符合即可

进程可以在前后台之间进行切换，要将一个前台进程切换到后台执行，可首先按`ctrl+Z`让正在后台执行的进程暂停。
然后用`jobs`获取当前的后台作业号，通过命令`bg 作业号`将进程放入后台执行,通过`fg 作业号`奖进程从后台调用到前台。

```shell
ps -ef
ps -ef &

jobs

bg 3
```

进程终止

```shell
# kill killall
# 信号代码 -9表示强制终止
kill -9 PID
```

进程的优先级

在Linux操作系统中，各个进程都使用资源，比如CPU和内存是竞争关系，这个竞争关系可以通过一个数值，人为地改变，
如进程的优先级较高，可以分配更多的时间片。优先级通过数字确认，负值或0表示高优先级，拥有优先占用系统资源的
权力。优先级的数值为`-20~19`,对应的命令为`nice和renice`

nice可以在创建进程时指定进程的优先级，进程优先级的值是父进程Shell优先级的值与所指定优先级的相加和，因此
使用nice设置程序优先级时，所指定数值是一个增量，并不是优先级的绝对值。

在启动一个进程时，其默认的优先级为0，可以通过nice命令来指定程序启动时的优先级，也可以通过renice来改变
正在执行的进程的优先级。

### 系统运维常见操作

Linux默认运行级别，当操作系统安装完成后，安装程序会按安装的定制选项为操作系统设置默认的运行级别，
通常安装有桌面时会设置为运行级别5，作为服务器运行时设置为3。通过命令systemctl可以修改默认的运行级别
```shell
#修改默认运行级别实际是修改文件default.target的指向。

ls -l /etc/systemed/system/default.target

#修改默认运行级别
systemctl set-default multi-user.target

```

sshd

安装完毕后, ssh默认端口一般为22，由于是通用定义的端口，若服务器上有外网开发，则可能存在风险，
通过一下步骤可更改ss的默认端口
```shell
#修改ssh配置文件
vi /etc/ssh/sshd_config
#重启sshd服务
systemctl restart sshd.service

netstat -ano | grep 22
```

```shell
#查看一个用户的所有进程
ps -ef | grep username
# 终止属于某个用户的所有进程
killall -u username

# 根据端口号查找对应进程
netstat -nlp | grep 22
```
