### NFS(Network File System)
NFS是一种分布式文件系统，即网络文件系统，允许网络中不同操作系统的计算机之间共享文件，其通信协议基于
TCP/IP协议层,可以把远程文件系统挂载在自己文件系统之下。

```shell
#把远程服务器192.168.3.101上的/nfsshare挂载到本地目录,挂载成功后，
#本地/nfsshare目录下若有数据，则原有数据不可见
mount 192.168.3.101:/nfsshare /nfsshare
```

NFS在文件传送或信息传送过程中依赖RPC(Remote Procdeure Call)协议，RPC协议可以在不同系统间使用，
此通讯协议设计与主机及操作系统无关。NFS本身可以认为是RPC的一个程序。只要用到NFS的地方就要启动RPC服务，
无论是服务端还是客户端，NFS是一个文件系统，而RPC负责信息的传输。

```shell

rpm -aq | grep nfs-utils

rpm -aq | grep rpcbind

#安装包
#nfs-utils-lib-1.1.5-6.el6.x86_64.rpm
#nfs-utils-1.2.3-39.el6.x86_64.rpm

#rpcbind-0.2.0-11.el6.x86_64.rpm
```

### 文件服务器Samba

利用Samba,Linux可以创建基于Window的计算机使用共享，另外，Samba还提供一些工具，
允许LInux用户从Windows计算机进入共享和传输文件，Samba基于Server Messages Block协议，
可以为局域网内计算机系统之间提供文件及打印机等资源的共享服务。

```shell

rpm -aq | grep samba

#samba4-libs-4.0.0-58.el6.rc4.x86_64
#samba-winbind-clients-3.6.9-164.el6.x86_64
#samba-client-3.6.9-164.el6.x86_64
#samba-common-3.6.9-164.el6.x86_64
#samba-winbind-3.6.9-164.el6.x86_64
```

### FTP服务器

FTP是File Transfer Protocol的缩写，文件传输协议，用于在网络上进行文件传输的一套标准协议，使用客户/服务器模式,
它属于网络传输协议的应用层。

FTP文件共享基于TCP/IP协议，是一种通用性比较强的网络文件共享方式。FTP方便地解决了
文件的传输问题，从而让人们可以方便地从计算机网络中获取资源。FTP已经成为计算机网路上
文件共享的一个标准。FTP服务器中的文件按目录结构进行组织，用户通过网络与服务器建立
连接。FTP仅基于TCP的服务，不支持UDP。与众不同的是FTP使用两个端口，一个数据端口(20)，
一个命令端口(21),也可以叫控制端口。

在Linux系统下，vsftpd(Very Secure FTP daemon)是一款应用比较广泛的FTP软件，小乔轻快，安全易用。
```shell
rpm -aq | grep vsftpd
#rpm包安装
rpm -ivh vsftpd-3.02-10.e17.x86_64.rpm
```

FTP软件除vsftpd之外，主要还有`proftpd、pureftpd和wu-ftpd`。
