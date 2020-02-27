ping 192.168.1.76

telnet 192.168.1.76 8080

查看端口是否占用
netstat -anp|grep 80

Linux 查看进程
ps -ef | grep node

检测是否已经安装过Vim
rpm -qa|grep vim

查看端口是否开放
```
lsof -i:8090
```
没有任何输出则说明没有开启该端口号

查看所有开启的端口号
```
netstat -aptn

netstat -nupl

netstat -ntpl
```

开放端口
```
8000端口

sbin/iptables -I INPUT -p tcp --dport 8000 -j ACCEPT
```
在命令行输入以上命令后，再输入/etc/rc.d/init.d/iptables save保存防火墙配置，

保存防火墙配置
```
/etc/rc.d/init.d/iptables save
```
保存配置后我们重启防火墙服务，服务就能生效,
重启防火墙服务
```
/etc/rc.d/init.d/iptables restart
```
查看安全服务是否开启
```
/etc/init.d/iptables status
```
关闭端口
```
iptables -A OUTPUT -p tcp --dport 8000 -j DROP
```



具体的操作是：/etc/rc.d/init.d/iptables restart。重启后我们可以输/etc/init.d/iptables status相应的安全服务是否开启



`[root@www ~]# tar [-j|-z] [cv] [-f 创建的档名] filename... <==打包与压缩`

`[root@www ~]# tar [-j|-z] [tv] [-f 创建的档名]             <==察看档名`

`[root@www ~]# tar [-j|-z] [xv] [-f 创建的档名] [-C 目录]   <==解压缩`



`-j  ：透过 bzip2 的支持进行压缩/解压缩：此时档名最好为 *.tar.bz2`


`-z  ：透过 gzip  的支持进行压缩/解压缩：此时档名最好为 *.tar.gz`



zip命令

将/home/Blinux/html/这个目录下所有文件和文件夹打包为当前目录下的html.zip：
```
zip -q -r html.zip /home/Blinux/html
```

上面的命令操作是将绝对地址的文件及文件夹进行压缩，以下给出压缩相对路径目录，比如目前在Bliux这个目录下，执行以下操作可以达到以上同样的效果：
```
zip -q -r html.zip html
```

比如现在我的html目录下，我操作的zip压缩命令是：
```
zip -q -r html.zip *
```

unzip命令

将压缩文件text.zip在当前目录下解压缩。
```
unzip test.zip
```

将压缩文件text.zip在指定目录/tmp下解压缩，如果已有相同的文件存在，要求unzip命令不覆盖原先的文件。
```
unzip -n test.zip -d /tmp
```

查看压缩文件目录，但不解压。
```
unzip -v test.zip
```

将压缩文件test.zip在指定目录/tmp下解压缩，如果已有相同的文件存在，要求unzip命令覆盖原先的文件。
```
unzip -o test.zip -d tmp/
```

ping命令 :包的四种故障解析

1、connect: Network is unreachable ： 网络不可达： 本机 路由表无法判定

2、Destination Host Unreachable ： 主机不可达： 局域网中无法找到对应IP的MAC地址，无法完成封装

3、destination net unreachable ： 来自于下一跳主机的回应， 本机将包转发给网关时，网关也无法到达目标网络

4、没有返回：对方无法返回，或者中间的转发设备丢弃了我们的包
