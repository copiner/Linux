### shell
/etc/shells

/etc/passwd

```
history

家目录内的 .bash_history
```

```
alias

ls -al

ls -l
```

为了方便 shell 的操作，其实 bash 已经`内建`了很多命令了
利用 type 这个命令来观察这个命令是来自于外部命令(指的是其他非 bash 所提供的命令) 还是内建在 bash 当中的。
```
type [-tpa] name

选项与参数：

    ：不加任何选项与参数时，type 会显示出 name 是外部命令还是 bash 内建命令

-t  ：当加入 -t 参数时，type 会将 name 以底下这些字眼显示出他的意义：

      file    ：表示为外部命令；

      alias   ：表示该命令为命令别名所配置的名称；

      builtin ：表示该命令为 bash 内建的命令功能；

-p  ：如果后面接的 name 为外部命令时，才会显示完整文件名；

-a  ：会由 PATH 变量定义的路径中，将所有含 name 的命令都列出来，包含 alias

```

```
type ls

type -t ls

type -a ls

type cd
```
利用` \[Enter] `来将` [Enter] `这个按键` 跳脱！`开来，让` [Enter] `按键不再具有` 开始运行 `的功能！好让命令可以继续在下一行输入。
