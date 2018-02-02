ctags安装成功后，要为源码文件生成tags文件，才可享受ctags为阅读代码带来的便利

`ctags -R`

递归的为当前目录及子目录下的所有代码文件生成tags文件

为某些源码生成tags文件，使用如下命令

`$ ctags filename.c filename1.c file.h `

或

`$ ctags *.c *.h`

为了使得字段补全有效，在生成tags时需要一些额外的参数，推荐的c++参数主要是：

`ctags -R --c++-kinds=+px --fields=+iaS --extra=+q`

其中：
选项c++-kinds 用于指定C++语言的 tags记录类型,  --c-kinds用于指定c语言的，  通用格式是  --{language}-kinds

选项 fileds 用于指定每条标记的扩展字段域

extra 选项用于增加额外的条目:   f表示为每个文件增加一个条目，  q为每个类增加一个条目

`使用方法`

在vim打开源码时，指定tags文件，才可正常使用，通常手动指定，在vim命令行输入：

`:set tags=./tags(当前路径下的tags文件)`

若要引用多个不同目录的tags文件，可以用逗号隔开

或者

设置 ~/.vimrc，加入一行，则不用手动设置tags路径：

`set tags=~/path/tags`

若要加入系统函数或全局变量的tag标签，则需执行：

`ctags -I __THROW –file-scope=yes –langmap=c:+.h –languages=c,c++ –links=yes –c-kinds=+p --fields=+S -R -f ~/.vim/systags /usr/include /usr/local/include`

并且在~/.vimrc中添加：

`set tags+=~/.vim/systags`

这样，便可以享受系统库函数名补全、原型预览等功能了

vim环境中其他较为好用的快捷键：

`* 定位至当前光光标所指单词的下一次出现的地方`

`# 定位至当前光光标所指单词的上一次出现的地方`

`n 定位至跳至已被标记出的单词下一次出现的地方`

`shift+n 定位至跳至已被标记出的单词上一次出现的地方`

关于更详细的ctags用法，vim中使用

`:help tags`
