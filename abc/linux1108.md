RPM(Redhat Package Manager)

用RPM安装软件包，最简单的命令如下：
```
rpm -i example.rpm 安装 example.rpm 包

rpm -iv example.rpm 安装 example.rpm 包并在安装过程中显示正在安装的文件信息

rpm -ivh example.rpm 安装 example.rpm 包并在安装过程中显示正在安装的文件信息及安装进度

```
删除已安装的软件包

```
rpm -e example
```

升级软件包
```
rpm -Uvh example.rpm
```

用户要注意的是：rpm会自动卸载相应软件包的老版本。如果老版本软件的配置文件通新版本的不兼容，rpm会自动将其保存为另外一个文件，用户会看到下面的信息：

```
saving /etc/example.conf as /etc/example.conf.rpmsave
```
这样用户就可以自己手工去更改相应的配置文件。
另外如果用户要安装老版本的软件，用户就会看到下面的出错信息：
```
rpm -Uvh example.rpm

examle packag example-2.0-l(which is newer) is already installed

error:example.rpm cannot be installed
```
如果用户要强行安装就使用-oldpackage参数。

查询软件包
```
rpm -q example
```
查询时可以使用的特定参数：
```
-a 　　：查询目前系统安装的所有软件包
-f 文件名 　　：查询包括该文件的软件包
-F 　　：同-f参数，只是输入是标准输入（例如 find /usr/bin | rpm -qF)
-q 软件包名 　　： 查询该软件包
```

用rpm校验软件包
```
rpm -Vf 需要验证的包
```


用户可以通过FTP来安装软件包。如果用户能够连上网络，想安装某个新的软件包时，可以直接用它的URL地址来安装：
比如：现在在ftp.linuxsir.com/pub/linux/redhat/RPMS/下有这个文件包：foo-1.0-1.i386.rpm，那就可以用这样的命令：


```
rpm -i ftp.linuxsir.com/pub/linux/redhat/RPMS/foo-1.0-1.i386.rpm
```

如果用户不小心误删了几个文件，但不确定到底是那些文件，想对整个系统进行校验，以了解哪些部分可能已经损坏，可以用：
```
rpm -Va
```

如果用户碰到一个认不出来的文件，想要知道它是属于那一个软件包的话，可以这样做：

```
rpm -qf /usr/X11R6/bin/xjewel

```
结果：
```
xjewel-1.6-1
```

如果用户得到一个新的RPM文件，却不清楚它的内容；或想了解某个文件包将会在系统里安装那些文件，可以这样做
```
rpm -qpi koules-1.2-2.i386.rpm
```


wget(World Wide Web get)是一种下载工具，通过HTTP、HTTPS、FTP三个最常见的TCP/IP协议下载，并可以使用HTTP代理

yum: 是redhat, centos 系统下的软件安装方式，基于Linux，全称为 Yellow dog Updater, Modified，是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器，基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包。



使用wget下载一个rpm包, 然后用 rpm -ivh  xxx.rpm  安装这个软件，也可以直接用  yum  install  XXX   来自动下载和安装依赖的rpm软件。
