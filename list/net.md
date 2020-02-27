### Linux网络管理

#### TCP/IP模型

1. 应用层:为用户提供所需要的各种应用服务，如FTP、Telnet、DNS、SMTP等
2. 传输层 ：为应用层提供端到端的通信功能，同时提供流量控制，确保数据完整和正确。
主要协议：TCP提供一种可靠的、面向链接的数据传输服务；
UDP提供不可靠的、无连接的数据报传输服务。

3. 网际互联层 ： 主要解决主机到主机之间的通信问题，
主要协议:网际协议(IP)、地址解析协议(ARP)、反向地址解析协议(ARP)、互联网控制报文协议(ICMP)
4. 网络接口层 ：主要为上层提供服务，完成链路控制等功能

#### 网络管理命令ping

```shell
ping

#ping指定发送包的次数
ping -c 1 192.168.23.100
#指定时间间隔和次数限制
ping -c 3 -i 0.01 192.168.3.100
#ping外网域名
ping -c 2 www.linux.org
```


测试目标主机或域名是否可达，通过发送ICMP数据包到网络主机，显示响应情况，并根据输出信息来确定目标主机或域名是否可达。
ping命令的结果通常是可信的，由于一些服务可以设置禁止ping，从而使ping的结果并不完全可信，ping命令常用参数如下：
```shell
-d : 使用Socket的SO_DEBUG功能
-f : 极限检测。大量且快速地传送网络封包给一台机器，看其回应。
-n : 只输出数值。
-q : 不显示任何传送封包的信息，只显示最后结果。
-r : 忽略普通的Routing Table, 直接将数据包发送到远端主机上。
-R : 记录路由过程。
-v : 详细显示指令的执行过程。
-c : 在发送指定数目的包后停止。
-i : 设置间隔几秒发送一个网络封包给一台机器，预设值是一秒发送一次。
......
```

#### 配置网络或显示网络接口状态

ifconfig命令可以用于查看、配置、启用或禁用指定网络接口，如配置网卡的IP地址、掩码、广播地址、网关等。
```shell
ifconfig interface [[-net -host]] address [parameters]
```
其中interface是网络接口名，address是分配给指定接口的主机名或IP地址。`-net和-host`参数分别告诉ifconfig
将这个地址作为网络号或是主机地址。
```shell
eth1      Link encap:Ethernet  HWaddr 00:50:56:90:D5:44  
          inet addr:192.168.23.213  Bcast:192.168.23.255  Mask:255.255.255.0
          inet6 addr: fe80::250:56ff:fe90:d544/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:117152617 errors:0 dropped:0 overruns:0 frame:0
          TX packets:24458511 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:14438226250 (13.4 GiB)  TX bytes:5557598983 (5.1 GiB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:16436  Metric:1
          RX packets:196083 errors:0 dropped:0 overruns:0 frame:0
          TX packets:196083 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:573091182 (546.5 MiB)  TX bytes:573091182 (546.5 MiB)
```
lo(loopback)为本地环回接口，IP地址固定为127.0.0.1，子网掩码8位，表示本机，virbr0是一个虚拟桥接网络，主要用于虚拟主机。

```shell
HWaddr : 是网路卡的硬体位址
inet addr : 是网路卡的 IP
Bcast : 是广播(broadcast) 的位址
Mask : 子网掩码
UP : 表示此网络接口为启用状态
RUNNING : 表示网卡设备已连接
MULTICAST : 表示支持组播
MTU : Maximum Trasmission Unit 最大传输单元(位元组), 即此介面一次所能传输的最大封包.
Metric : 是权值之意
RX packets : 接收到的包量，是个累计值。
TX packets : 发送的包量，是个累计值.
RX bytes : 表示接收的字节数.
TX bytes : 表示发送的字节数.
collisions : 是网路讯号碰撞的意思.
txqueuelen : 是传输缓区长度大小意思.
```

设置IP地址
```shell
ifconfig eno12453:5192.168.10.19 netmask 255.255.255.0 up

man ifconfig
```

#### 显示添加或修改路由表

route命令用于查看或编辑计算机的IP路由表。Route命令语法如下
```shell
route [-f] [-p] [command [destination] [mask netfmask] [gateway] [metric] [[dev] If]]
# command 指定想要进行的操作 如add、change、delete、print。
# destination 指定该路由的网络目标
# mask netmask 指定与网络目标相关的子网掩码
# gateway 为网关
# metric 为路由指定一个整体成本指标，当在路由表的多个路由中进行选择时可以使用
# dev if 为可以访问目标的网络接口指定接口索引
```

```shell
#显示所有路由表
route -n
#添加一条路由：发往192.168.60.0这个网段的全部要经过网关192.168.10.50
route add -net 193.168.60.0 netmask 255.255.255.0 gw 192.168.10.50
#删除一条路由，删除时不需网关
route del -net 192.168.60.0 netmask 255.255.255.0
```

```shell
Kernel IP routing table

Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
192.168.120.0   *               255.255.255.0   U     0      0        0 eth0
e192.168.0.0    192.168.120.1   255.255.0.0     UG    0      0        0 eth0
10.0.0.0        192.168.120.1   255.0.0.0       UG    0      0        0 eth0
default         192.168.120.240 0.0.0.0         UG    0      0        0 eth0

#第一行表示主机所在网络的地址为192.168.120.0，若数据传送目标是在本局域网内通信，则可直接通过eth0转发数据包;

#第四行表示数据传送目的是访问Internet，则由接口eth0，将数据包发送到网关192.168.120.240

#其中Flags为路由标志，标记当前网络节点的状态,Flags标志说明：

#U : Up表示此路由当前为启动状态
#H : Host，表示此网关为一主机
#G : Gateway，表示此网关为一路由器
#R : Reinstate Route，使用动态路由重新初始化的路由
#D : Dynamically,此路由是动态性地写入
#M : Modified，此路由是由路由守护程序或导向器动态修改
# ! : 表示此路由当前为关闭状态
```
#### 复制文件至其他系统

如果本地主机需要和远程主机进程数据迁移或文件传送，可以使用ftp或搭建web服务。另外可选的方法有
scp或rsync。

scp可以将本地文件传送到远程主机或从远程主机拉取文件到本地。
rsync赋值文件至其他系统，用于在不同的主机之间同步文件。
```shell
#参数
-P : 指定链接远程的端口
-q : 把进度参数关掉
-r : 递归复制整个文件夹

#将本地文件传送至远程主机172.16.15.70的/root路径下
scp -P 12345 nginx-1.2.9.tar.gz root@172.16.15.70:/root/
#将远程主机文件至本地
scp -P 12345 root@172.16.15.70:/root/nginx-1.2.9.tar.gz ./
#传送目录使用r参数
scp -P 12345 root@172.16.15.70:/root/nginx ./
scp -P 12345 nginx root@172.16.15.70:/root

man scp
```

#### 显示网络、路由表或接口状态

netstat用于监控系统网络配置和工作情况，可以显示内核路由表、活动的网络状态以及每个网络接口的有用的统计数字。
```shell
-a : 显示所有连接中的socket
-c : 持续列出网络状态
-h : 在线帮助
-i : 显示网络界面
-l : 显示监控中的服务器的Socket
-n : 直接使用IP地址
-p : 显示正在使用Socket的程序名称
-r : 显示路由表
-s : 显示网络工作信息统计表
-t : 显示TCP端口情况
-u : 显示UDP端口情况
-v : 显示命令执行过程
-V : 显示版本信息
```

```shell
#显示所有端口，包含UDP和TCP端口
netstat -a | head -4

#显示所有TCP端口
netstat -at

#显示所有UDP端口
netstat -au

#显示所有处于监听状态的端口并以数字方式显示而非服务名
netstat -ln | head

#显示所有TCP端口并显示所对应的进程或进程号
netstat -plnt

#显示核心路由信息
netstat -r

#显示网络接口列表
netstat -i
```

#### 探测至目的地址的路由信息
traceroute跟踪数据包到达网络主机所经过的路由，原理是试图以最小单位的TTL发出探测包来跟踪数据包
到达目标主机所经过的网关，然后监听一个来自网关ICMP的应答
```shell
ping www.linux.org

traceroute www.linux.org

traceroute -n www.mysql.com
```
#### 测试登陆或控制远程主机

telnet 命令常用来进行远程登陆，telnet程序是基于TELNET协议的远程登录客户端程序。
TELNET协议是TCP/IP协议族中的一员，是Internet远程登陆服务的标准协议和主要方式，
为用户提供了在本地计算机上完成远程主机工作的能力，在客户端可以使用telnet程序输入命令，
可以在本地控制服务器。由于telnet采用明文传送报文，安全性较差。

telnet可以确定远程服务端口的状态，以便确认服务是否正常。

```shell
#查找对应服务是否正常
telnet 192.168.3.100 8080

telnet www.mysql.com 80

man telnet
```

如果端口能正常telnet登录，则表示远程服务正常。除确认远程服务是否正常之外。对于提供开放telnet功能的服务，
使用telnet可以登录远程端口，输入合法的用户名和口令后，就可以进行其他工作了。

#### 下载网络文件

wget命令，同时支持FTP或HTTP协议下载，其参数说明如下
```shell
-b : 后台执行
-d : 显示调试信息
-nc : 不覆盖已有文件
-c : 断点续传
-N : 该参数指定wget只下载更新文件
-S : 显示服务器响应
...

# 下载文件
wget http://ftp.gnu.org/gnu/wget/wget-1.14.tar.gz
#断点续传
wget -c http://ftp.gnu.org/gnu/wget/wget-1.14.tar.gz


man wget
```

### Linux网络配置

#### Linux网络相关配置文件

```shell
# 计算机IP对应的主机名或域名对应的IP地址，通过设置/etc/nsswitch.conf中的选项可以选择是DNS解析优先还是本地设置优先
/etc/hosts
#DNS相关信息，用于将域名解析到IP
/etc/resolv.conf
# 网卡参数文件，比如IP地址、子网掩码、广播地址、网关等
/etc/sysconfig/network-scripts/ifcfg-*
#nsswitch.conf(Name Service Switch Configuration)：规定通过哪些途径、以及按照什么顺序来查找特定类型的信息。
/etc/nsswitch.conf
```

#### 配置Linux系统的IP地址
要设置主机IP地址，可以直接通过终端命令设置，如果想设置在系统重启后依然生效，可以通过设置对应网络接口文件来实现
```shell
cat /etc/sysconfig/network-scripts ifcfg-eth1

DEVICE=eth1
TYPE=Ethernet
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=none
IPV6INIT=no
USERCTL=no
IPADDR=192.168.23.213
NETMASK=255.255.255.0
GATEWAY=192.168.23.254
HWADDR=00:50:56:90:d5:44

DEVICE : 设备名
TYPE : 网络链接类型
ONBOOT : 系统启用时是否启用此网络接口
BOOTPROTO : 设置动态IP还是静态IP
GATEWAY : 默认路由
IPADDR : IP地址
NETMASK : 子网掩码
```

设置完ifcfg-eth1文件后，需要重启网络才能生效，重启后可以使用ifconfig查看是否生效
```shell
systemctl restart metwork
#或者
ifdown ifcfg-eth1
ifup ifcfg-eth1
```

#### 设置主机名
```shell
hostname

hostname mylinux

hostname

```
如果想设置在系统重启后依然生效，可以通过修改/etc/hosts文件，然后重启系统来实现
#### 设置默认网关
设置好IP地址以后，如果要访问其他的子网或Internet,用户还需要设置路由,设置默认网关
```shell
route add default gw 192.168.1.245
#或者修改/etc/sysconfig/network 文件添加以下字段,同样需要重启网络服务使其生效
GATEWAY=192.168.1.245

```
删除和添加设置默认网关

```shell
[root@localhost ~]# route del default gw 192.168.120.240  
[root@localhost ~]# route  
Kernel IP routing table  
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface  
192.168.120.0   *               255.255.255.0   U     0      0        0 eth0  
192.168.0.0     192.168.120.1   255.255.0.0     UG    0      0        0 eth0  
10.0.0.0        192.168.120.1   255.0.0.0       UG    0      0        0 eth0  
[root@localhost ~]# route add default gw 192.168.120.240  
[root@localhost ~]# route  
Kernel IP routing table  
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface  
192.168.120.0   *               255.255.255.0   U     0      0        0 eth0  
192.168.0.0     192.168.120.1   255.255.0.0     UG    0      0        0 eth0  
10.0.0.0        192.168.120.1   255.0.0.0       UG    0      0        0 eth0  
default         192.168.120.240 0.0.0.0         UG    0      0        0 eth0  
```


#### 动态主机配置协议DHCP

如果管理计算机有几十台，那么初始化服务器配置IP地址、网关、和子网掩码等参数是一个繁琐耗时的过程。
如果网络结构要更改，需要重新初始化网络参数，使用动态主机配置协议DHCP(Dynamic Host Confiuration Protocol)
则可以避免这个问题，客户端可以从DHCP服务端检索相关信息并完成相关网络配置，在系统重启后依然可以工作。

DHCP提供一种动态指定IP地址和相关网络配置参数的机制。动态主机配置协议用来自动给户端分配TCP/IP信息的网络协议，
如IP地址、网关、子网掩码等信息。每个`DHCP客户端`通过广播连接到区域内的`DHCP服务器`，该服务器会响应请求，返回包括
IP地址、网关和其他网络配置信息。

```shell
#查找是否安装DHCP服务
rpm -aq | grep dhcp

#DHCP客户端配置
cat /etc/sysconfig/network-scripts ifcfg-eth1

BOOTPROTO=dhcp

```

### 域名系统DNS

互联网应用越来越丰富，仅仅用IP地址标识网络上的计算机是不可能完成的任务，也没有必要，于是产生了域名系统。
用户按域名请求某个网络服务时，域名系统负责将其解析为对应的IP地址，这便是DNS。

目前网路上的域名服务系统使用最多的为BIND(Berkeley Internet Name Domain)软件，该软件实现了DNS协议。
```shell
rpm -aq | grep bind

rpm -ivh bind-9.9.4..x84_64.rpm

/etc/named.conf
/user/lib/systemd/system/named.service

/etc/init.d/named
/var/named/oa.com.zone

systemctl start named
```
#### 设置并测试DNS服务器
要设置DNS服务器，通常有两个方法，第一个方法是在接口配置文件中使用DNS1和DNS2指定
/etc/sysconfig/network-scripts ifcfg-eth1

第二个方法是修改`/etc/resolv.conf`文件。

查看DNS服务器,使用ping、nslookup或dig命令
```shell
nslookup www.mysql.com

ping www.mysql.com
```

### 监控网卡流量

监控网卡流量可以使用ifconfig或者查看系统文件/proc/net/dev中的数据，
其中`/proc/net/dev`文件中记录了不同网络接口(interface)上的各种包统计。

```shell
cat /proc/net/dev

Inter-|Receive                                                     |Transmit
face  |bytes      packets errs drop fifo frame compressed multicast|bytes     packets  errs drop fifo colls carrier compressed
lo:   574016731   197318    0    0    0     0          0         0  574016731  197318    0    0    0     0       0          0
eth1: 14467409808 117560201 0    0    0     0          0        48  5569450808 24533230  0    0    0     0       0          0

第一列是接口名称，lo(loopback接口)、eth1(Ethernet网卡)
第二列大列是这个接口接收统计
第三列大列是这个接口发送统计
```
