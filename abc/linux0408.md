### linux查看防火墙状态及开启关闭命令

存在以下两种方式：

一、service方式

查看防火墙状态：
```
[root@centos6 ~]# service iptables status

iptables：未运行防火墙。
```
开启防火墙：
```
[root@centos6 ~]# service iptables start
```
关闭防火墙：
```
[root@centos6 ~]# service iptables stop
```
二、iptables方式

先进入init.d目录，命令如下：
```
[root@centos6 ~]# cd /etc/init.d/

[root@centos6 init.d]#
```
然后

查看防火墙状态：
```
[root@centos6 init.d]# /etc/init.d/iptables status
```
暂时关闭防火墙：
```
[root@centos6 init.d]# /etc/init.d/iptables stop
```
重启iptables：
```
[root@centos6 init.d]# /etc/init.d/iptables restart
```
