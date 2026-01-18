# lec1: shell

[课程连接](https://missing-semester-cn.github.io/2020/course-shell/)<br>
[直接下载笔记(更新至lec2)](https://github.com/hubery258/Missing-Semester/archive/refs/heads/main.zip)<br>
[笔记仓库](https://github.com/hubery258/Missing-Semester)

## shell是什么？
可理解为与电脑进行交互的面板（接口），最流行的一种:bash

## shell入门

```
hubery@localhost:~$
```
`hubery`表示用户名，`localhost`是主机名，`~`表示当前工作目录是home,`$`表示以普通用户操作，`#`则表示root用户

### 一般的指令形式：
```
命令名称 [命令参数]
```
参数`--help`与`-h`等价，长格式是全称，短格式是单字母缩写，如果要写一个多单词对象：`"hello world"`或者`hello\ world`。

*查看帮助的方式：`名称 -h`或是`man 名称`(man即manual page)*

### 基础指令
- `cd`,`ls`,`mv`,`rm`,`cp`，`rmdir`,`mkdir`指令CS50X中提到过，不再详细写.

- `echo`可以直接输出一段字符串或变量的值(`echo $变量`)，如`echo hello`即可在terminal上打印hello

- `cat`可以读取**文件**内容并输出

- `touch`可以修改文件时间，当没有该文件时可以新建一个。(中文解释)[https://www.runoob.com/linux/linux-comm-touch.html]

- `pwd`获取当下所在路径，`cd /home`可以快速指定回到的目录，要记得cd后面那一坨都是参数，所以怎么给都可以，不要被cs50限制
    - 绝对路径就是完整的路径，其他都是相对路径
    - `.`表示当前目录，`..`表示上级目录
    - `cd -`可以回到前一个操作的目录，`cd ~`回到home目录

- 当使用`ls -l /home`时会输出详细内容，例如:
```
missing:~$ ls -l /home
drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing
``` 
第一个字符：`d`表目录，`-`普通文件，`l`是符号连接。后面每三个字符一组，第一组表示**所有者权限**(在这里就是missing),第二组是**用户组权限**(对应user),第三组是**其他人权限**,`w`对应可以写与修改，`x`对应可以执行，`r`对应可以阅读.

- 在需要时以super user(root)身份执行操作，需要使用`sudo`,放在程序的最前方来以sudo身份执行，但是注意`|`,`>`,`<`是通过shell执行的而不是被程序单独执行，英雌实际上`sudo echo 3 > brightness`推不动，需要`echo 3 | sudo tee brightness`

!!! tip 提效键
    - `ctrl+l`/输入`clear`可以清屏
    - `tab`可以自动填空，有多个选项是会给出选项，多填几个字再tab就行
    - `ctrl+c`强制退出
    - `↑`与`↓`可以载入上一条或下一条指令

*详细说明：但是，shell 是如何知道去哪里寻找 `date` 或 `echo` 的呢？其实，类似于 Python 或 Ruby，shell 是一个编程环境，所以它具备变量、条件、循环和函数（下一课进行讲解）。当你在 shell 中执行命令时，您实际上是在执行一段 shell 可以解释执行的简短代码。如果你要求 shell 执行某个指令，但是该指令并不是 shell 所了解的编程关键字，那么它会去咨询 `环境变量 $PATH`，它会列出当 shell 接到某条指令时，进行程序搜索的路径:*
```
missing:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
missing:~$ which echo (which给出echo所在的路径)
/bin/echo
missing:~$ /bin/echo $PATH (我们也可以绕过 $PATH，通过直接指定需要执行的程序的路径来执行该程序)
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

### 程序连接

在 shell 中，程序有两个主要的*stream*：*input stream*和*output stream*。 当程序尝试读取信息时，它们会从输入流中进行读取，当程序打印信息时，它们会将信息输出到输出流中。 默认(default)的输入输出流都是终端，但也可以重定向

最简单的重定向：`< file`(把输入流定向到file,即从file里读取数据), `> file`(输出),`>>` 可向文件追加内容:
```
missing:~$ echo hello > hello.txt
missing:~$ cat hello.txt
hello
missing:~$ cat < hello.txt
hello
missing:~$ cat < hello.txt > hello2.txt
missing:~$ cat hello2.txt
hello
```

更进一步，`|`使得我们可以把程序的输出与另一个程序的输入连接：`a | b`，a的输出作为b的输入，最终输出b的输出,这个管道(pipes)可以一直叠加。

## 课后练习
[官方解答](https://missing-semester-cn.github.io/missing-notes-and-solutions/2020/solutions/course-shell-solution/)<br>

`mkdir ./tmp/missing`即可解决第一步，不得不说`./tmp/missing`这么写是比原来先cd再创建方便哈

第9题：出现了一个`grep`，读了文档理解了：寻找与patterns一致的内容，然后把一整行输出，不给出文件就直接把当前文件夹全部扫描一遍，注意**Typically PATTERNS should be quoted when grep is used in a shell command.**：
```
hubery@localhost:/tmp/missing$ ./semester | grep "last-modified" > ~/last-modified.txt
```
注意主目录(`~`)与根文件夹`/`是不等价的

第10题：`cat < /sys/class/power_supply/BAT1/capacity`（Ubuntu 22.04.5的路径）可以获取电量百分比，哎这个题又不提前给电量信息路径在哪，记住前部分的路径吧，BAT1存电量各种数据的