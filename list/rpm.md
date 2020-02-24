### Linux软件包管理RPM(Redhat Package Manager)

RPM对应的管理命令为rpm，下面主要介绍通过rpm命令，安装、升级、卸载，软件包功能：

rpm 安装软件包
```shell

-i : 安装时显示软件包的相关信息
-v : 安装软件时显示命令的执行过程
-h : 安装软件时输出hash记号

# 安装lrzsz-0.12...x86_64.rpm

rpm -ivh lrzsz-0.12...x86_64.rpm

# 已经安装，因为某些原因想重新安装，可以采用强制安装
rpm -ivh --force lrzsz-0.12...x86_64.rpm

# 查看软件包文件列表及文件安装路径
rpm -qpl lrzsz-0.12...x86_64.rpm

-q : 使用询问模式，当遇到任何问题时，rpm指令先询问用户
-p : 查询软件包的文件
-l : 显示软件包的文件列表

# 如果安装软件时遇到相互依赖的软件包导致不能安装，可以使用nodeps参数先禁止检查软件包依赖以便完成安装
rpm -ivh --nodeps lrzsz-0.12...x86_64.rpm

```
rpm 升级软件包

```shell
# 更新已安装的软件
rpm -Uvh lrzsz-0.12...x86_64.rpm

-U : 升级指定软件

# 查看已安装的软件包
rpm -qa

-a : 显示安装的所有软件列表

rpm -qa | grep rz

```
rpm 卸载软件包

```shell
#卸载软件包
-e : 从系统中移除软件包

rpm -e lrzsz
```
其他
```shell
# 查看一个文件属于哪一个软件包
which rz

/usr/bin/rz

rpm -qf /usr/bin/rz

#查看rpm包的说明信息
rpm -qip lrzsz-0.12...x86_64.rpm
```

### 从源码安装软件


```shell
# 1.软件配置
./configure --prefix=/user/local/vim74
# 其中--prefix用来指定安装路径，编译好的二进制文件和其他文件将被安装到此处

#./configure --help

# 软件编译
make

# 软件安装
make install


```
一般情况下，安装完成后就可以使用安装的软件了，如果没有指定安装路径，一般的软件会安装在
`/user/loacl`下面，并创建对应的文件夹，软件部分二进制文件会被安装在`/usr/bin`或者
`/usr/local/bin`目录下，对应的头文件会被安装到`/usr/include`，软件帮助文档会被安装到
`/usr/local/share`目录下。

如果指定目录，则会在指定目录中创建相应的文件夹。安装软件完毕后使用该软件时需要使用绝对路径或者
`对环境变量进行配置`，也就是需要把当前软件二进制文件的目录加入到系统的环境变量PATH中。

### linux函数库
函数库是一个文件，它包含已经编译好的代码和数据，这些编译好的代码和数据可以供其他的程序调用。

程序函数库可以使程序更加模块化，更容易重新编译，而且方便升级。程序函数库分为3种类型：
`静态函数库、共享函数库、动态加载函数库`。

静态函数库(static libraries): 在编译程序时，如果指定了静态函数库文件，编译时会将这些静态函数库一起编译进最终的可执行文件中，这些库在程序执行前就加入到了目标程序中。

共享函数库(shared libraries): 在程序启动时加载到程序中，可以被不同的程序共享。

动态加载函数库(dynamically loaded libraries): 可以在程序运行的任何时候动态加载。

一般静态函数库以`.a`作为文件后缀。共享函数库中的函数是在当一个可执行程序启动时被加载，
一般动态函数库文件的扩展名为`.so`。

在Linux系统中，系统的静态函数库主要存放在`/usr/lib`目录下，而共享函数库文件主要存放在`/lib`
和`/usr/lib`目录下。动态函数库一般都是共享函数库。通常静态函数库只有一个程序使用，而共享函数库会被
多个程序使用。

在Linux系统中，如果一个函数库文件中的函数被某个文件调用，那么在执行使用了该函数库文件的程序时
必须要使执行程序能够找到函数库文件。系统通过两种方式来寻找函数库文件。

1. 通过缓存文件`/etc/ld.so.cache`。让系统在执行程序时可以从`ld.so.cache`文件中搜索到需要的库文件信息，
需要经过一下步骤。
```shell
# 首先修改/etc/ld.so.conf文件，将库文件所在的路径添加到文件中。
echo "/user/local/ssl/lib">>/etc/sd.so.conf
#然后执行ldconfig命令，让系统升级ld.so.cache
ldconfig
```
2. 通过环境变量`LD_LIBRARY_PATH`
```shell
export LD_LIBRARY_PATH=/usr/local/ssl/lib:$LD_LIBRARY_PATH:.
```
查看程序使用了哪些函数库，可以使用`ldd`命令。
```shell
ldd /usr/local/nginx/sbin/nginx
```
### 其他：
```
wget

yum
```
