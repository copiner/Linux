### 周期任务crond

```shell
crontab -l
```


Linux下的任务调度分为两类，系统任务调度和用户任务调度。系统任务调度：系统周期性所要执行的工作，比如写缓存数据到硬盘、日志清理等。在/etc/crontab文件，这个就是系统任务调度的配置文件。用户任务调度：用户定期要执行的工作，比如用户数据备份、定时邮件提醒等。用户可以使用 crontab 工具来定制自己的计划任务。在crontab 文件都被保存在/var/spool/cron目录中。其文件名与用户名一致。

crontab, cron来源于希腊语 chronos(χρόνος)，原意是时间，tab全称是table

`/etc/crontab`

```shell
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
HOME=/

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed

星号（*）：代表所有可能的值，如month字段为星号，则表示在满足其它字段的制约条件后每月都执行该命令操作。

逗号（,）：可以用逗号隔开的值指定一个列表范围，例如，"1,2,5,7,8,9"

中杠（-）：可以用整数之间的中杠表示一个整数范围，例如"2-6"表示"2,3,4,5,6"

正斜线（/）：可以用正斜线指定时间的间隔频率。

```



`/etc/cron.daily/`

```shell
*/30 * * * * /root/cron/delete_temp_file.sh >>/root/cron/del.log 2>&1
```
`/`——字符用来指定一个值的的增加幅度。比如在"秒"字段中设置为"0/15"表示"第0, 15, 30, 和 45秒"。而 "5/15"则表示"第5, 20, 35, 和 50"

对于& 1 更准确的说应该是文件描述符 1,而1标识标准输出，stdout。
对于2 ，表示标准错误，stderr。
2>&1 的意思就是将标准错误重定向到标准输出。这里标准输出已经重定向到了 /dev/null。那么标准错误也会输出到/dev/null

可以把/dev/null 可以看作"黑洞". 它等价于一个只写文件. 所有写入它的内容都会永远丢失. 而尝试从它那儿读取内容则什么也读不到

我们可以理解为，左边是标准输出，好，现在标准输出直接输入到 /dev/null 中，而2>&1是将标准错误重定向到标准输出，所以当程序产生错误的时候，相当于错误流向左边，而左边依旧是输入到/dev/null中。

注意：

```shell

shell上:
0表示标准输入
1表示标准输出
2表示标准错误输出
> 默认为标准输出重定向，与 1> 相同
2>&1 意思是把 标准错误输出 重定向到 标准输出.
&>file 意思是把 标准输出 和 标准错误输出 都重定向到文件file中

```

定时任务里面的程序脚本尽量用全路径！

### other

过滤文本：grep

文本操作：awk sed

打包压缩 : tar zip/unzip

查看系统负载：uptime

内存状态 ： free
