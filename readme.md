查看端口是否占用
netstat -anp|grep 80

Linux 查看进程
ps -ef | grep node

检测是否已经安装过Vim
rpm -qa|grep vim


`[root@www ~]# tar [-j|-z] [cv] [-f 创建的档名] filename... <==打包与压缩`

`[root@www ~]# tar [-j|-z] [tv] [-f 创建的档名]             <==察看档名`

`[root@www ~]# tar [-j|-z] [xv] [-f 创建的档名] [-C 目录]   <==解压缩`



`-j  ：透过 bzip2 的支持进行压缩/解压缩：此时档名最好为 *.tar.bz2`


`-z  ：透过 gzip  的支持进行压缩/解压缩：此时档名最好为 *.tar.gz`
