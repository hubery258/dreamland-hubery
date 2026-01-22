# lec4: 数据整理 

将某种格式存储的数据转换成另外一种格式,此处的格式转换更多指向**同类型仅转换输出的方式，使其更接近理想的格式**

`ssh myserver journalctl`可以实现打印服务器日志

`ssh myserver 'journalctl | grep sshd | grep "Disconnected from"' | less`,使用`''`把信息处理交给service side最后直接接收文件信息。`less`实现分页输出。


## 正则表达式(Regular expression) 

[正则表达式在线调试](https://regex101.com/r/qqbZqh/2)<br>
[在线教程](https://regexone.com/)

`sed` 是一个基于文本编辑器 `ed` 构建的 *stream editor* 。在 sed 中，可以用简短的命令来修改文件，而不直接操作文件的内容，例如`s`替换命令:`ssh myserver journalctl | grep sshd | grep "Disconnected from" | sed 's/.*Disconnected from //'`。语法是`s/REGEX/SUBSTITUTION/`, 其中 `REGEX` 部分是需要使用的**正则表达式**，而 `SUBSTITUTION` 是用于替换匹配结果的文本,没有就相当于删除效果。

正则表达式通常以（尽管并不总是）`/` 开始和结束,常见的有:

- `.` 除换行符之外的 “任意单个字符”
- `*` 匹配前面字符零次或多次
- `+` 匹配前面字符一次或多次
- `[abc]` 匹配 a, b 和 c 中的任意一个
- `(RX1|RX2)` 任何能够匹配 RX1 或 RX2 的结果
- `^` 行首
- `$` 行尾

但`*`与`+`在一般情况下保持贪婪模式：会**不断地匹配新的文本**，例如对下面进行操作:<br>
```
Jan 17 03:13:00 thesquareplanet.com sshd[2631]: Disconnected from invalid user Disconnected from 46.97.239.16 port 55920 [preauth]
```
我们希望对第一个Disconnected form作匹配，但是`*`会匹配到第二个Disconnected from，于是输出:`46.97.239.16 port 55920 [preauth]`,只需要给 `*` 或 `+` 增加一个 `?` 后缀使其变成非贪婪模式，但`sed`并不支持该后缀,可以切换到`perl` 的命令行模式,或直接给`sed`加入`-E`/`-r`后缀支持拓展：

``` bash
perl -pe 's/.*?Disconnected from //'
```

如果需要精准的筛出用户名，首先要:
```bash
 sed -E 's/.*Disconnected from (invalid |authenticating )?user .* [^ ]+ port [0-9]+( \[preauth\])?$//'
 ```
`[^ ]+` 会匹配任意**非空**且**不包含空格**的序列,最后的`\`可以理解为一种反义操作。而被**圆括号**内的正则表达式匹配到的文本，都会被存入一系列以编号区分的**捕获组**中。捕获组的内容可以在替换字符串时使用（有些正则表达式的引擎甚至支持替换表达式本身），例如 `\1`、 `\2`、`\3` 等等，因此可以使用如下命令：
```bash
| sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
```

## 数据整理进阶

整理出访问最多的用户:
```bash
ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
 | sort | uniq -c
```
`sort`会把整理的数据排序，`uniq -c` 会把连续出现(相邻)的行折叠为一行并使用出现次数作为前缀。

获取最多的十个人：
```bash
ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
 | sort | uniq -c
 | sort -nk1,1 | tail -n10
```

`sort -n`会按照数字顺序对输入进行排序（默认情况下是按照字典序排序 `-k1,1` 则表示“仅基于以空格分割的第一列进行排序”。,`tail -n` 部分表示“仅排序到第 n 个部分”，默认情况是到行尾。就本例来说，针对整个行进行排序也没有任何问题.

!!! tip inline end "字典序"
    *`sort -n`默认升序排序，字典序排序是指把数字也当字符串处理，`2`排在`10`前面;`-km, n`表示从m到n考虑排序。*

如果只想获取用户名，而且不要一行一个地显示:
```bash
ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
 | sort | uniq -c
 | sort -nk1,1 | tail -n10
 | awk '{print $2}' | paste -sd,
```
`paste` 命令可合并行(`-s`)，并指定一个分隔符进行分割 (`-d`)，那 awk 的作用又是什么呢？

## awk

`awk` 其实是一种编程语言，只不过它碰巧非常善于处理文本。`{print $2}` 的作用：`awk` 程序接受一个模式串（可选）和一个代码块，指定当模式匹配时该做何种操作。我们这里用的是默认的模式串，即匹配所有行。 在代码块中，`$0` 表示整行的内容，`$1` 到 `$n` 为一行中的 n 个**域**，域的分割基于 `awk` 的域分隔符（默认是空格，可以通过 `-F` 来修改）。在这个例子中，我们的代码意思是：对于每一行文本，打印其第二个部分，也就是用户名。

更复杂地:
```bash
| awk '$1 == 1 && $2 ~ /^c[^ ]*e$/ { print $2 }' | wc -l
```
可以匹配出现次数为1且以c开头以e结尾的username的，然后打印username，`wc -l` 统计输出结果的行数。(wc主要起统计功能)

**既然 `awk` 是一种编程语言**，那么则可以这样：
```bash
BEGIN { rows = 0 }
$1 == 1 && $2 ~ /^c[^ ]*e$/ { rows += $1 }
END { print rows }
```
`BEGIN` 也是一种模式，它会匹配输入的开头（ END 则匹配结尾）。然后，对每一行第一个部分进行累加，最后将结果输出。事实上，我们完全可以抛弃 `grep` 和 `sed` ，因为`awk`就可以[解决所有问题](https://backreference.org/2010/02/10/idiomatic-awk/)

## 分析数据

`| paste -sd+ | bc -l`可进行加法计算。

`R`也是一种编程语言，它非常适合被用来进行**数据分析**和**绘制图表**。需要[提前安装](https://www.r-project.org/)，主要是数据统计使用

## 整理二进制数据

虽然到目前为止我们的讨论都是基于文本数据，但对于二进制文件其实同样有用。例如我们可以用 `ffmpeg` 从相机中捕获一张图片，将其转换成灰度图后通过 SSH 将压缩后的文件发送到远端服务器，并在那里解压、存档并显示。
```bash
ffmpeg -loglevel panic -i /dev/video0 -frames 1 -f image2 -
 | convert - -colorspace gray -
 | gzip
 | ssh mymachine 'gzip -d | tee copy.jpg | env DISPLAY=:0 feh -'
```

## 课后习题

第一题有点漫长....不过效果挺好
第二题如果是wsl是没有该文件的，跳过。
第三题:表达式中后一个 `input.txt` 会首先被清空，而且是发生在前的。所以前面一个 `input.txt` 在还没有被 `sed` 处理时已经为空了。在使用正则处理文件前最好是首先备份文件。
第四题:wsl没有真正的开机文件，再次遗憾离场。
后面的题不想做了，以后需要数据处理的时候回来看。