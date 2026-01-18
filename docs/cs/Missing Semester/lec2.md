# lec2:shell脚本与工具

## Shell脚本语言
❓脚本语言：无需编译，解释器直接运行。

### 与C相近的语法

- 赋值:`foo=bar`，若有空格隔开会变为参数
- 字符串的定义：
    - `""`中可以实现替换变量值，`''`则不然：
    - `echo "$foo"`输出bar，而`echo '$foo'`输出$foo
- 命令执行时有 **两个输出流 (STDOUT/STDERR)** 用来打印信息，另外还有一个 **退出状态码** 用来告诉 shell 成功或失败。
    - STDERR负责输出错误信息比如`No such file or directory。`，可以单独重定向`2>`
    - 0是顺利运行，别的就是出错
- 退出码可以搭配 `&&`和 `||`使用，用来进行条件判断，决定是否执行其他程序,与C一致是短路运算符，eg:`false || echo "hello"`会输出hello，若`||`换成`&&`则不然。

??? note "增补语法"
    🔹 `if` 语法
    ```bash
    if [[ 条件 ]]; then
        命令1
        命令2
    elif [[ 其他条件 ]]; then
        命令3
    else
        命令4
    fi
    ```
    <br>
    **常见例子**：
    ```bash
    if [ -f "test.txt" ]; then
        echo "文件存在"
    else
        echo "文件不存在"
    fi
    ```
    <br>
    🔹 `for` 语法
    ```bash
    for i in {1..5}; do
        echo "第 $i 次"
    done
    ```
    也可以做c语言风格的循环

    🔹 `while` 语法
    ```bash
    while [[ 条件 ]]; do
        命令
    done
    ```


### 其他语法

<br>
函数示例：
```bash
mcd () {
    mkdir -p "$1"
    cd "$1"
}
```

和C中一致，`$0`表示脚本名，`$1`~`$9`是脚本的参数,剩下还有：

- `$@` - 所有参数
- `$#` - 参数个数
- `$?` - 前一个命令的返回值
- `$$` - 当前脚本的进程识别码
- `!!` - 完整的上一条命令，包括参数。常见应用：当你因为权限不足执行命令失败时，可以使用 `sudo !!` 再尝试一次。
- `$_` - 上一条命令的最后一个参数。<br>

示例二:
```bash
#!/bin/bash

echo "Starting program at $(date)" # date会被替换成日期和时间

echo "Running program $0 with $# arguments with pid $$"

for file in "$@"; do
    grep foobar "$file" > /dev/null 2> /dev/null
    # 如果模式没有找到，则grep退出状态为 1
    # 我们将标准输出流和标准错误流重定向到Null，因为我们并不关心这些信息,也就不需要保存或print
    if [[ $? -ne 0 ]]; then # -ne表示不等于
        echo "File $file does not have any foobar, adding one"
        echo "# foobar" >> "$file"
    fi
done
```

-  *命令替换（command substitution）*: `$(date)`先执行`date`然后其输出结果替换掉`$(date).`
    - 类似地有*进程替换（process substitution）*,`<( CMD )` 会执行 `CMD` 并将结果输出到一个临时文件中，并将 `<( CMD )` 替换成临时文件名。这在我们希望返回值通过文件而不是 STDIN 传递时很有用。例如， `diff <(ls foo) <(ls bar)` 会显示文件夹 `foo` 和 `bar` 中文件的区别。
- ``[[ ... ]]``是Bash的关键字，用于条件测试，类似于test命令或`[]`,但其更安全，所以一般用两层。


示例三:
```bash
convert image.{png,jpg}
# 有点像乘法分配律，实际会展开/等价为
convert image.png image.jpg

cp /path/to/project/{foo,bar,baz}.sh /newpath
# 会展开为
cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath

# 也可以结合通配使用
mv *{.py,.sh} folder
# 会移动所有 *.py 和 *.sh 文件

mkdir foo bar

# 下面命令会创建 foo/a, foo/b, ... foo/h, bar/a, bar/b, ... bar/h 这些文件，反正两个{}同时存在依次处理
touch {foo,bar}/{a..h}
touch foo/x bar/y
# 比较文件夹 foo 和 bar 中包含文件的不同
diff <(ls foo) <(ls bar)
# 输出
# < x
# ---
# > y
```

- *通配（globbing）*: 由 Shell 在执行命令前展开,作用于文件名匹配，类似于正则，但是正则作用于文本内容且由具体程序在运行时解析，还是有所差别。
    - `?`匹配任意单个字符
    - `*`匹配任意长度的任意字符（包括空字符串）。
    - `{foo,bar}`就会展开
<br>

脚本不是必须用bash写才能终端调用，只要写了(shebang)[https://en.wikipedia.org/wiki/Shebang_(Unix)] (开头第一行的`#!...`)就可以让内核明白用什么解释器运行，当然有一个好的做法是使用`env`(利用环境变量定位提高可移植性),于是`#!/usr/bin/env python`就指向了python解释器。

!!! tip inline end "函数与脚本"
    - 函数只能与 shell 使用相同的语言，脚本可以使用任意语言。因此在脚本中包含 `shebang` 是很重要的。
    - 函数仅在定义时被加载，脚本会在每次被执行时加载。这让函数的加载比脚本略快一些，但每次修改函数定义，都要重新加载一次。
    - 函数会在当前的 shell 环境中执行，脚本会在单独的进程中执行。因此，函数可以对环境变量进行更改，比如改变当前工作目录，脚本则不行。使用 [export](https://man7.org/linux/man-pages/man1/export.1p.html) 导出的环境变量会以传值的方式传递给脚本。
    - 与其他程序语言一样，函数可以提高代码模块性、代码复用性并创建清晰性的结构。shell 脚本中往往也会包含它们自己的函数定义。

## shell工具

当manual page显得有点臃肿时，可以使用[tldr](https://tldr.sh/)来快速了解实例.

### 查找文件
用`find`:
```bash
# 查找所有名称为src的文件夹
find . -name src -type d
# 查找所有文件夹路径中包含test的python文件
find . -path '*/test/*.py' -type f
# 查找前一天修改的所有文件
find . -mtime -1
# 查找所有大小在500k至10M的tar.gz文件
find . -size +500k -size -10M -name '*.tar.gz'
# 删除全部扩展名为.tmp 的文件
find . -name '*.tmp' -exec rm {} \;
# 查找全部的 PNG 文件并将其转换为 JPG
find . -name '*.png' -exec magick {} {}.jpg \;
```

在`find`里`-exec`后的`{}`表示find找到的文件，`\;`表示命令结束，每找到一个执行一次.当然还有更友好的[fd](https://github.com/sharkdp/fd)工具可以使用,自行研究。

!!! abstract inline end "Shell的哲学"
    shell 的哲学之一便是寻找（更好用的）替代方案。 记住，shell 最好的特性就是您只是在调用程序，因此您只要找到合适的替代程序即可（甚至自己编写）。而且重要的是你要知道有些问题使用合适的工具就会迎刃而解，而具体选择哪个工具则不是那么重要。

### 查找代码

自带的工具是`grep`,其中`-C`：获取查找结果的上下文（Context）；`-v` 将对结果进行反选（Invert），也就是输出不匹配的结果。举例来说， `grep -C 5` 会输出匹配结果前后五行。当需要搜索大量文件的时候，使用 `-R` 会递归地进入子目录并搜索所有的文本文件。
<br>

其他工具：[ack](https://beyondgrep.com/),[ag](https://github.com/ggreer/the_silver_searcher),[rg](https://github.com/BurntSushi/ripgrep)，提供了一些rg示例，有待实践。
```bash
# 查找所有使用了 requests 库的文件
rg -t py 'import requests'
# 查找所有没有写 shebang 的文件（包含隐藏文件）
rg -u --files-without-match "^#\!"
# 查找所有的foo字符串，并打印其之后的5行
rg foo -A 5
# 打印匹配的统计信息（匹配的行和文件的数量）
rg --stats PATTERN
```

### 查找自己的shell命令
`history`输出所有历史命令，`history 23`可以指定一共要输出多少条（从末尾往前数），配合`history | grep ...`可能是更佳的选择
??? tip "history 进阶"
    你可以修改 shell history 的行为，例如，如果在命令的开头加上一个空格，它就不会被加进 shell 记录中。当你输入包含密码或是其他敏感信息的命令时会用到这一特性。 为此你需要在 .bashrc 中添加 `HISTCONTROL=ignorespace` 或者向 `.zshrc` 添加 `setopt HIST_IGNORE_SPACE`。 如果你不小心忘了在前面加空格，可以通过编辑 `.bash_history` 或 `.zhistory` 来手动地从历史记录中移除那一项。）

也可以`Ctrl+R`对历史记录回溯搜索，可以配合模糊查找工具[fzf](https://github.com/junegunn/fzf/wiki/Configuring-shell-key-bindings#ctrl-r)使用.

### 文件夹导航

- 快速切换以及常用访问：[fasd](https://github.com/clvv/fasd)
- 概览目录结构: 自带的`tree`,[nnn](https://github.com/jarun/nnn?tab=readme-ov-file)

## 课后练习

- 第一题：**在 Linux 命令里，短选项可以组合在一起。**,`ls -a -clt --color=auto -h`即可解决问题，`-a`表示全部显示，`-clt`是短选项组合，`-c`是排序，`-lt`是按时间排序，`--color=auto`则控制颜色，`-h`是自动human-readable.
<br>

**📂 最安全的地方**

- 你的 home 目录：
    - 路径通常是 /home/<用户名>，在 WSL2 Ubuntu 下就是 /home/hubery。
    - 这里是你个人的工作区，写文件、测试脚本都不会影响系统。
    - 用 cd ~ 就能快速进入。
- 临时目录 /tmp：
    - 这是专门用来存放临时文件的地方。
    - 系统会定期清理，不会影响核心文件。
    - 适合做一些一次性的测试。

- 第二题： 
```bash
#!/bin/bash

marco() {
    MARCO="$(pwd)" # 直接创造变量
    echo "Saved current directory: $MARCO"
}

polo() {
    if [[ -n "$MARCO" ]]; then # -n的意思是 -n STRING the length of STRING is nonzero
        cd "$MARCO" || echo "Failed to cd into $MARCO"
    else
        echo "No directory saved yet. Run marco first."
    fi
}
```
<br>
或者

```bash
#!/bin/bash
 marco(){
     echo "$(pwd)" > $HOME/marco_history.log
     echo "save pwd $(pwd)"
 }
 polo(){
     cd "$(cat "$HOME/marco_history.log")"
 }
```

注意这样写的时候，因为是定义的函数，所以运行时直接输入`marco`运行就好了，不是脚本。

第三题：摸着示例过河，注意经常要先用`chmod`给出运行脚本权限
```bash
#!/usr/bin/env bash

cnt=0
echo > out.log

while true
do
    ./bug.sh >> out.log 2>> out.log
    if [[ $? -ne 0 ]]; then #两边都有空格
        echo "done $cnt times"
        cat out.log
        exit 0
    fi
    cnt=$((cnt+1)) # cnt=$((cnt+1))/((cnt++)),不太懂原理，语法还得另看
done 
```

第四题：有`tldr`就是爽
```bash
find . -name '*.html' | xargs -0 tar cvzf html_file.tar.gz
```

拓展：不如前面难，查查文档即可搞定`find . -type f -print0 | xargs -0 ls -lt | head -1`