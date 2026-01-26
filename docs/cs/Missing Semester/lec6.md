# lec6: git

*说明: 部分具体指令在cs50x中学了，所以只写不会的部分，补充内容见系列笔记目录中的git*

版本控制系统: *VCSs*

## 数据模型

在 Git 的术语里，文件被称作 `Blob` 对象(数据对象)。目录则被称为`tree`，它将名字与 Blob 对象或树对象进行映射（使得目录中可以包含其他目录）。快照则是被追踪的最顶层的树,是一种*commit*。例如，一个树看起来可能是这样的：
```
<root> (tree)
|
+- foo (tree)
|  |
|  + bar.txt (blob, contents = "hello world")
|
+- baz.txt (blob, contents = "git is wonderful")
```

所有快照用hash值来标记，Git 给这些哈希值赋予人类可读的名字，也就是*引用（references）*。引用是指向提交的指针。与对象不同的是，它是可变的（引用可以被更新，指向新的提交）。例如，`master` 引用通常会指向主分支的最新一次提交。

### 仓库
Git 仓库：`对象` 和 `引用`。

在硬盘上，Git 仅存储对象和引用：因为其数据模型仅包含这些东西。所有的 git 命令都对应着对提交树的操作，例如增加对象，增加或删除引用。

## git的命令行接口

见单独的[git笔记](git.md)

## 课后习题

第二题:
1. `git clone ...`
2. `git log | grep -A 5 -B 5 "README.md"`查找到`seven_bear`,查`man git-log`改进：`git log --follow README.md`,人没变。
3. `git blame _config.yml`直接确认时间最晚的，然后把hash值复制到`git show ...`的...处可直接看到内容
???+ info "对show内容的解释"
    ```bash
    $ git show bcd3b8e6
    commit bcd3b8e6da24d3a8ea4e4c888f6852a7d5ce67d6
    Author: Lingfeng_Ai <hanxiaomax@gmail.com>
    Date:   Fri May 28 20:20:48 2021 +0800

    Update _config.yml

    diff --git a/_config.yml b/_config.yml
    index 522fa29..6d9b90c 100644
    --- a/_config.yml
    +++ b/_config.yml
    @@ -1,7 +1,7 @@
    # Setup
    title: 'the missing semester of your cs education'
    url: https://missing-semester-cn.github.io
    -solution_url: /2020/solutions/
    +solution_url: missing-notes-and-solutions/2020/solutions/
    # Settings
    markdown: kramdown
    kramdown:
    ```
    diff开始是对文件的对比，`@@ -1,7 +1,7 @@` 表示修改部分的上下文范围（旧文件从1开始，显示7行，对应新文件从1开始，显示7行），`-`是旧版本删除内容，`+`是新版本增加内容，其余是未修改的上下文，帮助理解修改
    - 草，审错题了，可以直接git blame文件之后就按当行内容直接show,注意commit内容太多时要`git show ... -- <文件名>`直接锁定。