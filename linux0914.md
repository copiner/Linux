CentOS（Linux）下如何安装源码包

一、下载源码包，解压，可使用 wget 下载到主机 (系统预设源代码保存位置 /usr/local/src)

二、进入文件，查看 INSTALL README

三、软件配置与软件,环境依赖检查

(1)如果有configure ./configure 或者 .configure，生成makefile文件。

./configure --prefix=/usr/local/指定安装根目录

.--with指的是安装本文件所依赖的库文件。

/configure是源代码安装的第一步，主要的作用是对即将安装的软件进行配置，检查当前的环境是否满足要安装软件的依赖关系，生成makefile文件

(2)如果有makefile，就直接make，然后make install

四、make 是用来编译 它从Makefile中读取指令，然后编译。

编译出错可以用make clean 清除编译过程文件，make install是用来安装的，它也从Makefile中读取指令，安装到指定的位置。只有在执行install的时候才会向指定的安装目录写入文件。

系统预设软件安装位置 /usr/local

注意：

安装过程报错 ：编译配置过程停止并出现error warning no。一般Leaving directory...表示成功。

源码包是没有卸载命令，直接执行rm命令卸载即可，没有垃圾残留
