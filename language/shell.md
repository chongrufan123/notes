# shell
命令解释器，将外层应用解释为机械码  
也是一个编程语言，可以直接调用linux系统命令

<!-- vim-markdown-toc Marked -->

* [shell语言的分类](#shell语言的分类)
    * [查看系统支持的shell语言](#查看系统支持的shell语言)
* [shell脚本的执行方法](#shell脚本的执行方法)
    * [echo输出命令](#echo输出命令)
    * [第一个脚本](#第一个脚本)
    * [执行脚本](#执行脚本)
* [Bash的基本功能](#bash的基本功能)
    * [历史命令和补全](#历史命令和补全)
        * [history](#history)
        * [历史命令的调用](#历史命令的调用)
    * [命令别名和常用快捷键](#命令别名和常用快捷键)
        * [alias](#alias)
        * [命令执行循序](#命令执行循序)
        * [常用快捷键](#常用快捷键)
    * [输入输出重定向](#输入输出重定向)
        * [标准输入输出](#标准输入输出)
        * [输出重定向](#输出重定向)
        * [输入重定向](#输入重定向)
    * [多命令顺序执行与管道符](#多命令顺序执行与管道符)
        * [多命令执行符](#多命令执行符)
        * [管道符](#管道符)
    * [通配符和特殊符号](#通配符和特殊符号)
        * [通配符](#通配符)
        * [其他特殊符号](#其他特殊符号)

<!-- vim-markdown-toc -->

## shell语言的分类
1. Bourne
- sh
- ksh  
- Bash(linux中用的,和sh兼容)
- psh
- zsh
2. C
- csh
- tcsh

### 查看系统支持的shell语言
```
cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash
```
直接在交互框中输入shell的名字就可以切换shell 

## shell脚本的执行方法
### echo输出命令
```
echo [选项] [输出内容]
```
选项
- -e    支持反斜线控制的字符转化

|控制字符|作用|
|:---:|:----|
|\|输出\|
|\a|输出警告声|
|\b|退格键|
|\c|取消输出行末的换行符|
|\e|ESCAPE键|
|\f|换页键|
|\n|换行|
|\r|回车|
|\t|tab|
|\v|垂直制表符|
|\0nnn|按八进制ascll输出|
|\xhh|按16位ascll输出|

```
echo -e "\e[1;31m abcd \e[0m"   //\e[1;开启颜色输出,31m红色
```

### 第一个脚本
开头必须要由
```
#!/bin/Bash
```
以sh为后缀

### 执行脚本
1. 赋予权限,直接执行(dui)
```
chmod 755 hello.sh
./hello.sh
```
2. 通过Bash调用执行脚本
```
bash hello.sh
```
## Bash的基本功能
### 历史命令和补全
#### history
history [选项][历史命令保存文件]  
选项  
- -c 清空历史命令  
- -w 把缓存中的历史命令写入文件~/.bash_history

#### 历史命令的调用
- 上下箭头调用之前的历史命令
- !n重复调用第n调命令
- !! 重复调用上一条命令
- !字串 重复调用最后一条该字串开头的命令

### 命令别名和常用快捷键
#### alias
```
alias   //查询别名
alias 别名='原来命令'   //设置别名
unalias 别名    //删除别名
```
在/root/.bashrc中改变可以让别名永久生效
#### 命令执行循序
1. 绝对路径和相对路径执行的命令
2. 执行别名
3. Bash的内部命令
4. \$PATH环境变量定义的第一个命令

#### 常用快捷键
|快捷键|作用|
|:---:|:-----|
|ctrl+A|光标移动到命令行开头|
|ctrl+E|光标移动到命令行结尾|
|***ctrl+C***|强制终止当前命令|
|***ctrl+L***|清屏|
|***ctrl+U***|删除剪切光标之前的内容|
|ctrl+K|删除剪切光标之后的内容|
|***ctrl+Y***|粘贴|
|***ctrl+R***|在历史命令中搜索|
|***ctrl+D***|退出终端|
|ctrl+Z|暂停并放入后台|
|ctrl+S|暂停屏幕输出|
|ctrl+Q|回复屏幕输出|

### 输入输出重定向
#### 标准输入输出
|设备|设备文件名|文件描述符|类型|
|:---:|:---:|:---:|:---:|
|键盘|\/dev\/stdin|0|标准输入|
|显示器|\/dev\/sdtout|1|标准输出|
|显示器|\/dev\/sdterr|2|标准错误输出| 

#### 输出重定向
|类型|符号|作用|
|:----|:----|:----|
|标准输出重定向|命令 > 文件|覆盖,正确输出|
| |命令 >> 文件|追加,正确输出|
|标准错误输出重定向|错误命令 2> 文件|覆盖,错误输出|
| |错误命令 2>> 文件|追加,错误输出|
|正确输出和错误输出同时保存|命令 > 文件 2>&1|覆盖,错误正确同时|
| | 命令 >> 文件 2>&1|追加,错误正确同时|
| | 命令 &> 文件|覆盖,错误正确同时|
| | 命令 &>> 文件|追加,错误正确同时|
| | 命令 >> 文件1 2>>文件2|正确到文件1,错误到文件2|

#### 输入重定向
```
wc 选项 文件名  //统计文件中的***, 按<c-d>结束
```
选项
- -c 统计字节数
- -w 统计单词数
- -l 统计行数 
```
命令<文件   //对文件执行命令
```

### 多命令顺序执行与管道符
#### 多命令执行符
|多命令执行符|格式|作用|
|:----|:----|:----|
|;|命令1;命令2;...|无任何关系,并行执行几条命令|
|&&|命令1 && 命令2|命令1正确执行才会执行命令2|
|\|\||命令1 \|\| 命令2|命令1不正确执行才会执行命令2|

```
dd if=输入文件 of=输出文件 bs=字节数 count=个数
选项
if=输入文件 //指定源文件或原设备
of=输出文件 //指定目标文件或目标设备
bs=字节数   //指定一次传递多少字节,把这些字节看作是一个数据块
count=个数  //指定传入多少数据块

date ; dd if=/dev/zero of=testfile bs=1k count=100000 ; date   //测试磁盘速度

//执行结果
2021年 02月 24日 星期三 22:41:36 CST
记录了100000+0 的读入
记录了100000+0 的写出
102400000 bytes (102 MB, 98 MiB) copied, 1.42232 s, 72.0 MB/s
2021年 02月 24日 星期三 22:41:37 CST
```
```
./configure && make && make install //配置加编译加编译安装
命令 && echo yes || echo no //判断命令是否正确执行
```

#### 管道符
```
命令1 | 命令2   //命令1的正确输出作为命令2的操作对象
ll -a /etc/ | more  //分屏显示命令输出内容
netstat -an | grep "ESTABLISHED"    //查找当前正在登录服务器的对象
```
```
grep 选项 "搜索内容" 文件名 //将文件中关键字所在行提取出来
选项
-i  忽略大小写
-n  输出行号
-v  反向查找
--color=auto    //搜索出来的关键字用颜色显示
```
### 通配符和特殊符号
#### 通配符
|通配符|作用|
|:----|:----|
|?|匹配一个任意字符|
|*|匹配零个,一个或多个任意字符|
|[]|匹配中括号中任意一个字符|
|[-]|匹配中括号中任意一个字符,-表示范围|
|[^]|逻辑非|

#### 其他特殊符号
|符号|作用|
|:----|:----|
|''|单引号,在其内任何符号没有特殊含义|
|""|双引号,在其内有特殊含义|
|\`\`|反引号,括起来的内容会执行系统命令|
|\$()|表示括号内的内容是系统命令|
|#|该行注释|
|\$|用来调用变量的值|
|\\|转义符|


