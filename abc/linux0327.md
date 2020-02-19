### 权限

在Linux里面，任何一个文件都具有User, Group及Others三种身份的个別权，还有万能权限的root

在我們Linux系統当中，默认的情況下，所有的系統上的帳號與一般身份使用者，
還有那個root的相關信息， 都是記錄在/etc/passwd這個檔案內的。
至於個人的密碼則是記錄在/etc/shadow這個檔案下。此外，Linux所有的群組名稱都紀錄在/etc/group內！
這三個檔案可以說是Linux系統裡面帳號、密碼、群組資訊的集中地

```
r read
w write
x execute

  user group  others    user group
   |     |      |        |     |
- rwx   r-x   r--.  1   root root 660 8月  20 2018 app.js


-rwxr-xr--

[-][rwx][r-x][r--]
 1  234  567  890

 1 為：代表這個檔名為目錄或檔案，本例中為檔案(-)；
234為：擁有者的權限，本例中為可讀、可寫、可執行(rwx)；
567為：同群組使用者權限，本例中為可讀可執行(rx)；
890為：其他使用者權限，本例中為可讀(r)，就是唯讀之意
```

### 改變檔案屬性與權限

```
chgrp ：改變檔案所屬群組(change group)
chown ：改變檔案擁有者(change owner)
chmod ：改變檔案的權限, SUID, SGID, SBIT等等的特性
```


檔案權限的改變使用的是chmod這個指令，但是，權限的設定方法有兩種， 分別可以使用數字或者是符號來進行權限的變更.

#### 數字類型改變檔案權限

Linux檔案的基本權限就有九個，分別是owner/group/others三種身份各有自己的read/write/execute權限， 先複習一下剛剛上面提到的資料：檔案的權限字元為：『-rwxrwxrwx』， 這九個權限是三個三個一組的！其中，我們可以使用數字來代表各個權限，各權限的分數對照表如下：

```
r:4
w:2
x:1

chgrp [-R] 帳號名稱 檔案或目錄

chown [-R] 帳號名稱 檔案或目錄

```

每種身份(owner/group/others)各自的三個權限(r/w/x)分數是需要累加的，例如當權限為： [-rwxrwx---] 分數則是：

```
owner = rwx = 4+2+1 = 7
group = rwx = 4+2+1 = 7
others= --- = 0+0+0 = 0

chmod [-R] xyz 檔案或目錄

chmod -R 770 app.js

chmod 755 test.sh
chmod 740 test.sh
```

#### 符號類型改變檔案權限

```
u(user)
g(group)
o(others)
a(all)

+(加入)
-(除去)
=(設定)

r (read)
w (write)
x (execute)
```

```
chmod  u=rwx,go=rx  .bashrc

chmod  a+w  .bashrc

chmod  a-x  .bashrc
```
如果你在某目錄下不具有x的權限， 那麼你就無法切換到該目錄下，也就無法執行該目錄下的任何指令，即使你具有該目錄的r或w的權限.

要開放目錄給任何人瀏覽時，應該至少也要給予r及x的權限，但w權限不可隨便給

如果該目錄屬於用戶本身,那麼使用者在這個目錄底下能夠刪除檔案


####

### 用户


#### 查看linux所有用户
通过使用 /etc/passwd 文件，getent 命令，compgen 命令这三种方法查看系统中用户的信息。
```
cat /etc/passwd

getent passwd

compgen -u
```
大家都知道，Linux 系统中用户信息存放在 /etc/passwd 文件中。/etc/passwd 文件将每个用户的基本信息记录为文件中的一行，一行中包含 7 个字段。
每个字段之间用冒号 : 分隔：

```
# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
tcpdump:x:72:72::/:/sbin/nologin
2gadmin:x:500:10::/home/viadmin:/bin/bash
apache:x:48:48:Apache:/var/www:/sbin/nologin
zabbix:x:498:499:Zabbix Monitoring System:/var/lib/zabbix:/sbin/nologin
mysql:x:497:502::/home/mysql:/bin/bash
zend:x:502:503::/u01/zend/zend/gui/lighttpd:/sbin/nologin
rpc:x:32:32:Rpcbind Daemon:/var/cache/rpcbind:/sbin/nologin
2daygeek:x:503:504::/home/2daygeek:/bin/bash
named:x:25:25:Named:/var/named:/sbin/nologin
mageshm:x:506:507:2g Admin - Magesh M:/home/mageshm:/bin/bash

```

```
7 个字段的详细信息如下。

用户名 （magesh）： 已创建用户的用户名，字符长度 1 个到 12 个字符。
密码（x）：代表加密密码保存在 `/etc/shadow 文件中。
**用户 ID（506）：代表用户的 ID 号，每个用户都要有一个唯一的 ID 。UID 号为 0 的是为 root 用户保留的，UID 号 1 到 99 是为系统用户保留的，UID 号 100-999 是为系统账户和群组保留的。
**群组 ID （507）：代表群组的 ID 号，每个群组都要有一个唯一的 GID ，保存在 /etc/group文件中。
**用户信息（2g Admin - Magesh M）：代表描述字段，可以用来描述用户的信息（LCTT 译注：此处原文疑有误）。
**家目录（/home/mageshm）：代表用户的家目录。
**Shell（/bin/bash）：代表用户使用的 shell 类型。
```

#### 切换用户

```
格式为: su [ - ] username，后面可以跟 - ，也可以不跟。

普通用户的su命令不加username时，就相当于切换到root用户，反之亦然。当su 命令加上 - 后，会初始化当前用户的各种环境变量。
如果不加 - 切换到root用户时，当前目录没有变化；而如果加上 - 切换到root账户时，当前目录为root账户的家目录。

当由root切换到普通用户时，不需要输入密码

```
