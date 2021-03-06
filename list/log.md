### Linux常见日志文件
`/var/log`

日志类型：

系统连接日志:

 `/var/log/wtmp`; `/var/run/utmp`;`/var/run/lastlog`;`/var/run/secure`

wtmp与utmp为二进制文件，不能通过文本查看命令直接查看，
可以通过who、w、users、last、、lastlog、ac
查看这两个文件的信息; who命令查询utmp文件并报告当前登录的每个用户。
who命令的输出包含用户名、终端类型、登陆


进程统计日志 `/var/log/messages`;

错误日志 `/var/log/messages`; `/var/log/dmesg`;

可以使用命令`dmesg|grep -i error`查看比如硬盘损坏或其他故障。

### Linux日志系统
实际的日常管理中，每天的日志量非常大，在进行排查或跟踪时，使用grep查看日志文件是件痛苦的事情。
于是，syslog的替代品rsyslog出现了，Redhant使用rsyslog替换syslog。

rsyslog日志系统：

rsyslog默认配置文件为`/etc/rsyslog.conf`;
该配置文件定义了系统中需要监听的事件和对应的日志文件的保存位置，示例如下：

```shell
*.info;mail.none;authpriv.none;cron.none          /var/log/messages
...
```
设备本身为两个字段，小数点的前一段表示一项服务，后面一个字段表示优先级。

服务：
```shell
auth : 由pam_pwdb报告的认证活动
authpriv : 包括特权信息（如用户名）在内的认证活动。
daemon : 与inetd守护进程有关的后台进程信息。
kern :  内核信息。
mail : 与电子邮件有关的信息。
......
```

优先级
```shell
* : 所有级别，除了none
none : 没有重要级别，通常用于排错
info : 提供信息的消息
notice : 具有重要性的普通条件
warn : 预警信息
alert : 需要立即修改的条件
......
```
### 使用日志轮转

使用系统提供的logrotate功能:

logrotate命令: logrotate [option] <configfile>

e.g.
```shell
logrotate -vf /etc/logrotate.conf
```
可以通过man logrotate查看更多帮助信息

logrotate配置文件：`/etc/logtotate.conf`;`/etc/logrotate.d`

可以通过查看配置文件了解更多，比如了解更多配置文件参数信息。
