IED hd  硬盘hda hdb hdc     硬盘hda分区  hda1 hda2 hda3  .......
SATA SCSI sd 硬盘sda sdb sdc 硬盘sda分区 sda1 sda2 sda3  .......

盘符
/
挂载


ls -al
ls--help


date +%Y
date +%y
date +%M
date +%m

cal 日历
cal 2012
cal 12 2012


在命令终端中可以通过Ctrl+r 实现快速检索使用过的历史命令。Ctrl+r中r是retrieve中r。

Ctrl+a：光标回到命令行首。 （a：ahead）

Ctrl+e：光标回到命令行尾。 （e：end）

Ctrl+b：光标向行首移动一个字符。 （b：backwards）

Ctrl+ f：光标向行尾移动一个字符。 （f：forwards）

Ctrl+w: 删除光标处到行首的字符。

Ctrl+k：删除光标处到行尾的字符。

Ctrl+u：删除整个命令行文本字符。

Ctrl+h：向行首删除一个字符。

Ctrl+d：向行尾删除一个字符。




bc 计数器   
	scale=4 //保留4位小数
exit

光标回到本行开头
ctrl+ a


光标回到本行结尾
ctrl + e


引用上一个命令的字符
按一下esc ，然后按下.(点）。

touch创建文本文件。

touch xx
执行touch ^xx^yy

中间滚轮，执行复制粘贴


查看某个选项的帮助，结合 --help更好理解命令
whatis命令
例如：
whatis pwd


shell变量
变量名=值
echo &xx  输出xx

read

array数组

declare
ulimit

alias 别名



通配符
[a-z]匹配其中一个字符
?表示匹配一个（仅一个）任意字符
*任意字符零个或多个
[^0-9]非数字

正确重定向 ">"  ">>"
拿日历(cal)举例
cal > text 输出到text文件，若没有则创建，否则清空内容再写入。
cal >> text 输出到text文件内容后面。

错误重定向
"2>"
"2>>"

不管正确与错误重定向
"&>"
"&>>"   //???????有错误

其它
cal 2> text 1>&2
ss > text 2>&1



"<"  "<<"

tr 'a-z' 'A-Z' < text


hell文档


判断语句
  ; && ||

执行多个命令分号；隔开
date ; cal



管道pipe  "|"
选取命令 cut， grep(过滤）
cut -d: -f1 test  查看test以：分割的第一部分f1

last | cut -d: -f1   last命令结果通过管道传给cut命令

排序命令：sort, wc, uniq

sort
sort -n

sort -t: -k3 test   //第三部分按字符排序
sort -t: -k3 -n test   //第三部分按数字排序

last | cut -d"  "  -f1 | sort -u

last | cut -d"  "  -f1 | sort | uniq -c


wc(word count)



双重定向 tee

字符转换命令：tr, col, join, paste, expand
 col/expand 把Tab转换为空格



切割命令： split

参数代换： xargs

减号-

正则表达式
. 一个字符
^开头
$结束

o{4}表示4个o字符

o{1, 8}表示1个到8个o字符

sed

sed '1,2d' passwd    删除第一行和第二行，在内存并输出，不保存到文件

sed -i '1,2d' passwd  删除第一行和第二行并保存到文件

sed '/too/ipppp' passwd 再含有too的上一行添加pppp
sed '/too/apppp' passwd 再含有too的下一行添加pppp
sed '/too/cpppp' passwd 再含有too的一行替换为pppp

sed -e 's/too/tung/g' 替换
sed -e 's/too/tung/g' -e 's/xiao/tung/g' xx  //多个替换

也可以把这些命令放在一个文件下，比如yy 内容如下
s/too/tung/g
s/xiao/tung/g
/too/cpppp

然后执行
sed -f yy xx
达到同样的效果


awk 找到文件 排版

awk -F: '{}' xx  

awk '{print NR, $0, NF}' xx
awk '$3<=1 {print NR, $0, NF}' xx

awk -F: 'BEGIN{OFS="\t\t"; ORS="\n\n\n"}{print $1, $2}" yy



Shell script
数字运算
declare -i aa
let bb=1+8
cc=$((5+8))

返回值
执行正确返回值为0 错误返回值为非零数字
echo $?
数字比较
表示等于  -eq
表示大于  -gt
表示大于等于 -ge
表示小于  -lt
表示小于等于 -le

test [ ]
test $aa -eq $bb

字符串比较
> == <

判断语句

if ["$age" -le 0 ]|| ["$age" -ge 120 ]; than
	echo "unbelieveable"
elif ["$age" -gt 0 ]&&["$age" -lt 18 ]; than
	echo "child"
else
	echo "people"


case...esac
select
function xx(){}
xx(){}

while
until
for in


SSH
secure shell



备份dump
完全备份

增量备份
差异备份
