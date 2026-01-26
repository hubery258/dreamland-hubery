# GIT brief introduction from cs50
[course link](https://www.youtube.com/watch?v=MJUJ4wbFm_A&t=26s)

## ❓what is git and what can it do for me?
- 代码/文件管理工具
- Keeps track of changes to code
- Test changes to code without losing the original
- Revert back to old versions of code.

## git 基础指令
- `git init`初始化仓库(*repositories*)
- `git add <filename>` 提交一个文件到缓冲区:
    - `git add .` 全部提交
- `git diff`显示所有未add的修改
    - `git diff <filename>`显示与暂存区文件的差异
    - `git diff <version> <filename>`显示某个文件两个版本之间的差异
- `git commit -m "message" ` 把缓冲区的内容提交到分支：
    - `git commit -am "message" `可实现add和commit两步合并
    - `-am` 会把已被 Git 跟踪（tracked）的已改文件自动暂存并提交，但不会包括未被跟踪（untracked/new）文件。要提交新文件仍需先 `git add`。
- `git status` 仓库状态，你现在在哪个branch，文件情况
- `git branch` 显示所有branch
    - `git branch <name>` 创建新的branch
    - `git checkout <name>` 跳转到对应branch
    - `git checkout -b <name>`同时完成上面两个操作
    - `git branch -D <name>`删除分支

**merge conflict example**:
```
<<<<<<<<<<HEAD
int b = 2;  （你的修改）
============
int b = 0;  (别人的修改)
>>>>>>>>>> 哈希值，标识提交
```
只需要删去一条，比如只保留`int b = 2;`或者把两条按自己的方式合并一下。

- `git log`查看提交等等日志
    - `git log --all --graph --decorate`可视化历史记录
- `git reset`回滚：
    - `git reset --hard <commit>` 输入需要的提交hash值，可在log里查到
    - `git reset --hard origin/master` 回滚到remote branch
- `git merge <name>`把某一分支与当前所在分支merge，merge到当前这个分支
    - `git mergetool`使用工具处理合并冲突
- `pull request`对他人的库修改后提交的request，希望能merge，见github。 


## 远端操作
- `git remote`列出远端
    - `git remote add <name> <url>`添加远端
- `git clone <url>`，从远程仓库中copy一份，存在自己的电脑里
- `git pull`把线上仓库更改下载到本地仓库，可能出现
    - `git fetch`从远端获取对象/索引+`git merge`等价于`git pull`
- `git push`提交到远程仓库（没有访问权的话必须要先pull request）
    - `git push <remote> <local branch>:<remote branch>`将对象传送至远端并更新远端引用

## 撤销
- `git commit --amend`: 编辑提交的内容或信息
- `git reset HEAD <file>`: 恢复暂存的文件,`HEAD`是指分支名
- `git checkout -- <file>`: 丢弃修改
- `git restore`: git2.32 版本后取代 `git reset` 进行许多撤销操作

[其他操作](https://missing-semester-cn.github.io/2020/version-control/#:~:text=Git%20%E9%AB%98%E7%BA%A7%E6%93%8D%E4%BD%9C,%E8%BF%BD%E8%B8%AA%E7%9A%84%E6%96%87%E4%BB%B6)<br>
[git其他资源](https://missing-semester-cn.github.io/2020/version-control/#:~:text=%E8%B5%84%E6%BA%90,%E6%9D%A5%E5%AD%A6%E4%B9%A0%20Git%20%EF%BC%9B)
