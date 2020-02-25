### 系统的启动控制
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

top命令监视系统的实时状态
```shell
top

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
8 root      RT   0     0    0    0 S  0.0  0.0   0:00.00 migration/1
9 root      20   0     0    0    0 S  0.0  0.0   0:29.03 ksoftirqd/1
10 root     RT   0     0    0    0 S  0.0  0.0   0:22.77 watchdog/1

```
