# lec5 : Command-line Environment

总的来讲，用中学大于直接学，这一讲很多内容vscode可以解决或是暂时用不上，看看就好，做了练习不用还是忘了，需要时再去实践，学。

## 任务与进程控制

当我们输入 `Ctrl-C` 时，shell 会发送一个 `SIGINT` 信号到进程,进程就停止；我们也可以使用 `SIGQUIT` 信号，通过输入 `Ctrl-\` 可以发送该信号。

`SIGSTOP` 会让进程暂停。在终端中，键入 `Ctrl-Z` 会让 shell 发送 `SIGTSTP` 信号，可以用`fg`(前台运行)和`bg`(后台运行)恢复暂停的工作

`jobs`会列出当前终端中尚未完成的任务，可使用*pid*引用任务或`%任务编号`形式，任务编号出现在jobs中，`%!`快速选择最近一个任务

还有一件事情需要掌握，那就是命令中的 `&` 后缀可以让命令在直接在后台运行，这使得您可以直接在 shell 中继续做其他操作，不过它此时还是会使用 shell 的标准输出，这一点有时会比较恼人（这种情况可以使用 shell 重定向处理）。

一旦您关闭终端（会发送另外一个信号 `SIGHUP`），这些后台的进程也会终止。为了防止这种情况发生，您可以使用 `nohup`（一个用来忽略 `SIGHUP` 的封装）来运行程序。

## 别名

使用`alias`达到个性化设计功能:
``` bash
alias ll="ls -lh"
alias gs="git status"
alias sl=ls #打错了也识别
alias mv="mv -i"           # -i prompts before overwrite 重新定义一些命令行的默认行为
alias la="ls -A"
alias lla="la -l" # 别名可以组合使用
unalias la # 禁用别名
alias ll # 获取别名定义
```

在默认情况下 shell 并不会保存别名。为了让别名持续生效，您需要将配置放进 shell 的启动文件里，像是 `.bashrc` 或 `.zshrc`.

## 配置文件(Dotfiles)

大多配置文件以`.`开头，默认是隐藏文件，`ls`默认不显示。配置bash可编辑`.bashrc`

不同的配置文件里要放什么需要自己访问manual pages或者在线文档(当然这个时代ai可能可以帮忙)，或是网上搜索别人的配置。

## 远端设备

连接示例:`ssh foo@bar.mit.edu`,`foo`是用户名，后面是服务器，服务器可以URL指定也可以通过IP指定:`foobar@192.168.1.42`

### 执行命令

`ssh foobar@server ls`可以直接远程执行命令

### SSH密钥

基于密钥的验证机制使用了密码学中的公钥，我们只需要向服务器证明客户端持有对应的私钥，而不需要公开其私钥。这样您就可以避免每次登录都输入密码的麻烦了秘密就可以登录。

生成密钥可用[`ssh-keygen`](http://man7.org/linux/man-pages/man1/ssh-keygen.1.html)实现

### 通过 SSH 复制文件

使用 ssh 复制文件有很多方法：

- `ssh+tee`, 最简单的方法是执行 `ssh` 命令，然后通过这样的方法利用标准输入实现 `cat localfile | ssh remote_server tee serverfile`。`tee` 命令会将标准输出写入到一个文件；
- `scp` ：当需要拷贝大量的文件或目录时，使用 `scp` 命令则更加方便，因为它可以方便的遍历相关路径。语法如下：`scp path/to/local_file remote_host:path/to/remote_file`；