### Linux用户管理

用户账户文件：`/etc/passwd`

用户密码文件：`/etc/shadow`

用户组文件：`/etc/group`

### 用户管理命令

Linux用户分为3类：超级用户、系统用户(bin,daemon,mail)、普通用户

```shell
useradd

# -d : 指定用户登陆是的起始目录，不指定，默认为/home;
# e.g. :
# useradd wdaonngg
# useradd -d /home/wdaonngg wdaonngg

# other : `-g;-G;-m;-M;-s;-u`

userdel

# userdel username 删除用户,主目录不删除

# userdel -r username 连带删除用户主目录

usermod

# -d : 修改用户主目录
# other : `-e;-f;-g;-G;-l;-L;-s;-u;-U`

passwd 更改或设置用户密码

# passwd : 修改当前用户密码

# passwd username ： 修改指定用户密码


su 切换用户

# 不加参数，默认切换到root用户

sudo 普通用户获取超级权限
# 参数 -g;-b;-h;-k;-;...
#root用户可以授权普通用户执行特定的命令，登记信息文件为 /etc/sudoers
```

### 用户组管理命令
linux提供了一系列的命令管理用户组。用户组就是具有相同特征的用户集合。每个用户都有一个用户组，系统
能对一个用户组的所有用户进行集中管理，可以把相同属性的用户定义到同一用户组，并赋予该用户组
一定的操作权限，这样用户组下的用户对该文件或目录都具备了相同的权限。通过对`/etc/group`文件
的更新实现对用户组的添加、修改和删除。

一个用户可以属于多个组，`/etc/passwd`中定义的用户组为基本组，用户所属的组有基本组和附加组。
如果一个用户属于多个组，则该用户所拥有的权限是它所在的组的权限之和。
```shell
groupadd 添加用户组
# 参数：-g;-o;-p;-r

# groupadd groupname

# useradd -g groupname username

groupdel 删除用户组

groupmod 修改用户组

# -g 修改用户组ID; -n 修改用户组名字

用户所属用户组可以通过文件`/etc/passwd`查看，也可以通过命令
# 首先通过id命令查看用户信息
id username

grep username /etc/passwd

grep gid(group id) /etc/group
```



other
```shell
chown : change ower
chgrp : change group

chmod : change mode
```
