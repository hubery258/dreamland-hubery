# GIT brief introduction
[cs50 course link](https://www.youtube.com/watch?v=MJUJ4wbFm_A&t=26s)

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
- `git push`提交到远程仓库
    - `git push <remote> <local branch>:<remote branch>`将对象传送至远端并更新远端引用
    - `git push -u origin xx` 本地分支`xx`第一次传到远端
        - `-u`(`--set-upstream`)把`xx`分支推送到远程，同时把本地xx与远程建立追踪关系，以后就可以直接git push

## 撤销
- `git commit --amend`: 编辑提交的内容或信息
- `git reset HEAD <file>`: 恢复暂存的文件,`HEAD`是指分支名
	- 工作区有未暂存内容强制回退：` git reset --hard HEAD #永久删除工作区和暂存区的所有未提交修改`
- `git checkout -- <file>`: 丢弃修改
- `git restore`: git2.32 版本后取代 `git reset` 进行许多撤销操作
	- `git restore --source=<目标版本> <文件路径>`,`<目标版本>：可以是 Commit Hash（如 `a1b2c3d`）、分支名（如 `main`）、相对位置（如 `HEAD~3` 表示往回数 3 个版本）
	- 不记得hash: `git log --oneline -- <文件路径>`
- `git revert HEAD`: 已经提交了一次，回退一次

!!! note "reset不同操作"

	| 命令 | 仓库历史指针 | 暂存区（Index） | 工作区（文件） | 适用场景 |
	| :--- | :--- | :--- | :--- | :--- |
	| `git reset --soft <提交ID>` | ✅ 回退 | ❌ 不动（保留原改动） | ❌ 不动 | 想把最近几次提交合并成一个，重新提交 |
	| `git reset --mixed <提交ID>`<br>（默认） | ✅ 回退 | ✅ 清空（回到未暂存状态） | ❌ 不动 | 想把之前提交的改动撤回来，保留代码慢慢改 |
	| `git reset --hard <提交ID>` | ✅ 回退 | ✅ 清空 | ✅ **强行覆盖** | 彻底不要这之后的代码了，本地和仓库完全变成旧版 |

	**举例**：想彻底回到 3 个提交前的状态（所有文件都变成那时候的样子）：
	```bash
	git reset --hard HEAD~3
	```
	> 🚨 **致命警告**：如果这些提交已经推送到远程仓库，并且别人拉取过，**绝对不要用 `git reset --hard`**，否则别人下次推送时会和你冲突得一塌糊涂，协作项目直接崩盘。

[其他操作](https://missing-semester-cn.github.io/2020/version-control/#:~:text=Git%20%E9%AB%98%E7%BA%A7%E6%93%8D%E4%BD%9C,%E8%BF%BD%E8%B8%AA%E7%9A%84%E6%96%87%E4%BB%B6)<br>
[git其他资源](https://missing-semester-cn.github.io/2020/version-control/#:~:text=%E8%B5%84%E6%BA%90,%E6%9D%A5%E5%AD%A6%E4%B9%A0%20Git%20%EF%BC%9B)

## 更多日用补充

!!! note "`.gitignore`基本用法"
    - 直接一行一行输入你要git忽略的文件即可
    - `*`作为通配符依然成立
    - 如果是忽略所有文件中的`__pycache__/`文件，就直接写`__pycache__/`
    - 忽略根目录下的文件示例: `/debug.log`
    - 忽略`hw01/ok`这个文件(相对路径): `hw01/ok`
    - 忽略任意深度的 `tests` 目录：`**/tests/`
    - 取反,不忽略这个文件（例外）: `!important.pyc`

注意已经追踪的文件不会自动被忽略，移除索引操作:

- `git rm -r --cached __pycache__/`: `--cached` 表示只从 git 移除，不删本地文件
- `git rm -r --cached .`: 可以先直接把所有文件都从index中移除，然后再用add重新添加即可
    - `-r`是recursive,对于目录操作的时候需要递归，逻辑是删目录时 git 默认拒绝（怕你误删一堆文件），加 `-r` 表示"我知道这是个目录，递归删掉里面所有东西",这个逻辑在shell命令里通用规则，比如`rm -r dir/`等等

