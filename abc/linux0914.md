CentOS（Linux）下如何安装源码包

一、下载源码包，解压，可使用 wget 下载到主机 (系统预设源代码保存位置 /usr/local/src)

二、解压后查看INSTALL与README文件，这两个文件中详细介绍了本软件的安装方法和注意事项

三、软件配置与软件,环境依赖检查

(1)如果有configure ./configure 或者 .configure，执行configure命令，生成Makefile文件。

./configure --prefix=/usr/local/指定安装根目录

.--with指的是安装本文件所依赖的库文件。

/configure是源代码安装的第一步，主要的作用是对即将安装的软件进行配置，检查当前的环境是否满足要安装软件的依赖关系，生成makefile文件

(2)如果有makefile，就直接make，然后make install

四、make 是用来编译 它从Makefile中读取指令，然后编译。

make clean命令用来清除上一次编译生成的目标文件，编译出错可以用make clean 清除编译过程文件，

make install是用来安装的，它也从Makefile中读取指令，安装到指定的位置。只有在执行install的时候才会向指定的安装目录写入文件。

系统预设软件安装位置 /usr/local

注意：

安装过程报错 ：编译配置过程停止并出现error warning no。一般Leaving directory...表示成功。

源码包是没有卸载命令，直接执行rm命令卸载即可，没有垃圾残留


Linux上几乎所有的软件都经过了GPL授权，因此几乎所有的软件都会提供源码。 而一个软件要在Linux上执行，必须是二进制文件，因此当我们拿到软件源码后，需要将它编译成二进制文件才能在Linux上运行。

将源码编译成可供Linux运行的二进制文件一共需要两步：

1. 使用gcc编译器将源码编译成目标文件

2. 再次使用gcc编译器将目标文件链接成二进制文件

这过程看似简单，实则不然。一个软件的源代码往往被封装在多个源文件中，此外这些文件有错综复杂的依赖关系，编译需要严格按照指定的顺序进行，这无疑增加了编译的难度。好在make命令可以帮助我们简化编译过程。

整个编译过程被浓缩在Makefile文件中(告诉make命令需要怎么去编译和链接程序)，当执行make命令时，make会去当前目录中寻找Makefile文件，并根据该文件中的要求完成整个编译过程。

而Makefile文件由configure命令产生。当执行configure命令时，configure会根据当前系统环境动态生成一个适合本系统的Makefile文件，供make命令使用。
