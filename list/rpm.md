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
