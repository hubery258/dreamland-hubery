# lec3: Vim
???+ note "学习使用新的编辑器基本步骤"
    - 阅读教程（比如这节课以及我们为您提供的资源）
    - 坚持使用它来完成你所有的编辑工作（即使一开始这会让你的工作效率降低）
    - 随时查阅：如果某个操作看起来像是有更方便的实现方法，一般情况下真的会有,在前20个小时可能感觉效率降低，但之后就会提高效率。

## Vim的基本操作

???+ abstract inline end "Vim的哲学"
    在编程的时候，你会把大量时间花在阅读/编辑而不是在写代码上。所以，Vim 是一个 **多模态** 编辑 器：它对于插入文字和操纵文字有不同的模式。Vim 是可编程的（可以使用 Vimscript 或者像 Python 一样的其他程序语言），Vim 的接口本身也是一个程序语言：键入操作（以及其助记名） 是命令，这些命令也是可组合的。Vim 避免了使用鼠标，因为那样太慢了；Vim 甚至避免用 上下左右键因为那样需要太多的手指移动。

### 不同模式

- 正常(*normal*)模式：在文件中四处移动光标进行修改
- 插入(*insert*)模式：插入文本
- 替换模式：替换文本
- 可视化(*visual*)模式（一般，行，块）：选中文本块
- 命令模式：用于执行命令

在不同的操作模式下，键盘敲击的含义也不同，通常大部分时间都会处于正常和插入模式。按下 `<ESC>`（退出键）从任何其他模式返回正常模式。在正常模式，键入`i`进入插入模式，`R`进入替换模式，`v`进入可视（一般）模式，`V`进入可视（行）模式，`<C-v>` （Ctrl-V, 有时也写作 ^V）进入可视（块）模式，`:`进入命令模式。

### 基本操作
Vim 会维护一系列打开的文件，称为“缓存”(*buffer*),Vim可以显示多个标签页，每个标签页包含一系列窗口(分隔面板)，每个窗口展示一个缓存，**但是不同窗口可以展示同一个缓存，就可以同时看到一个文件的多个地方，**
在键入`:`后，你的光标会立即跳到屏幕下方的命令行:

- `:q` 退出（关闭窗口）
- `:w` 保存（写）
- `:wq` 保存然后退出
- `:e` {文件名} 打开要编辑的文件
- `:ls` 显示打开的缓存
- `:help {标题}` 打开帮助文档
- `:help :w` 打开 `:w` 命令的帮助文档
- `:help w` 打开 `w` 移动的帮助文档

## Vim 的界面本身是一个程序语言

Vim的界面本身是程序语言，意味着不同的键位产生的命令效果可以像函数一样堆叠使用，形成肌肉记忆之后会更高效。

### 移动
正常模式下，在 Vim 里面移动也被称为 “名词”， 因为它们指向文字块。:

- 基本移动: `hjkl` （左， 下， 上， 右）
- 词： `w` （下一个词,word）， `b` （词初,beginning）， `e` （词尾,end）
- 行： `0` （行初）， `^` （第一个非空格字符）， `$` （行尾）
- 屏幕： `H` （屏幕首行）， `M` （屏幕中间）， `L` （屏幕底部）
- 翻页： `Ctrl-u` （上翻）， `Ctrl-d` （下翻）
- 文件： `gg` （文件头）， `G` （文件尾）
- 行数： :`{行数}<CR>` 或者 `{行数}G` ({行数}为行数),跳转到某行
- 杂项： `%` （找到配对，比如括号或者 /* */ 之类的注释对）
- 查找： `f{字符}`(向右查找当前行内的第一个匹配字符)， `t{字符}`(右查并定位到其前一个字符)， `F{字符}`(左查)， `T{字符}`(左查并定位到后一个)
- `,` / `;` 用于导航匹配,`;`重复上一次查找,`,`反方向重复
- 搜索: `/{正则表达式}`向下搜索，`?{}`向上, `n` / `N` 用于导航匹配，`n`下一个结果，`N`上一个

### 选择
可视化模式:

- 可视化：`v`
- 可视化行： `V`
- 可视化块：`Ctrl+v`
可以用移动命令来选中,其实就是鼠标左键拖动之后选择文字的效果，但用可视化块可以到达选中间一块的效果。

### 编辑
所有你需要用鼠标做的事， 你现在都可以用键盘：采用编辑命令和移动命令的组合来完成。 这就是 Vim 的界面开始看起来像一个程序语言的时候。Vim 的编辑命令也被称为 “动词”， 因为动词可以施动于名词。

- `i` 进入插入模式
- `O` / `o` 在之上/之下新建行
- `d{移动命令}` 删除 {移动命令}
- 例如，`dw` 删除词, `d$` 从当前位置删除到行尾, `d0` 从...删除到行头。
- `c{移动命令}` 改变 {移动命令}
- 例如，cw 改变词,等价于先`dw`再`i`，c与d的区别在于c会自己进入插入模式
- `x` 删除字符（等同于 dl）
- `s` 替换字符（等同于 xi）
- 可视化模式 `+` 操作
- 选中文字, `d` 删除 或者 `c` 改变
- `u` 撤销(Undo), `<C-r>` 重做
- `y` 复制 / “yank” （其他一些命令比如 d 也会复制）
- `p` 粘贴
更多值得学习的: 比如 `~` 改变字符的大小写

### 计数

你可以用一个计数来结合“名词”和“动词”，这会执行指定操作若干次。

- `3w` 向后移动三个词
- `5j` 向下移动 5 行
- `7dw` 删除 7 个词

### 修饰语
你可以用修饰语改变“名词”的意义。修饰语有 i，表示“内部”或者“在内”，和 a， 表示“周围”。

- `ci(` 改变当前括号内的内容
- `ci[` 改变当前方括号内的内容
- `da'` 删除一个单引号字符串， 包括周围的单引号

## 自定义Vim
Vim 有一个位于 ~/.vimrc 的文本配置文件（包含 Vim 脚本命令）。你可能会启用很多基本设置。

[下载课程设置](https://missing-semester-cn.github.io/2020/files/vimrc),然后将它保存成 `~/.vimrc`.


!!! tip "将Win中下载的vimrc文件放到WSL中"

    1. 打开 WSL 终端（例如 Ubuntu）。
    2. 假设你在 Windows 上下载的文件路径是 `C:\Users\Jun\Downloads\vimrc`。在 WSL 中它对应的路径是：  
    `/mnt/c/Users/Jun/Downloads/vimrc`
    3. 在 WSL 中执行：
    ```bash
    cp /mnt/c/Users/Jun/Downloads/vimrc ~/.vimrc
    ```


Vim 能够被重度自定义，花时间探索自定义选项是值得的。授课人设置:[Anish](https://github.com/anishathalye/dotfiles/blob/master/vimrc),[Jon](https://github.com/jonhoo/configs/blob/master/editor/.config/nvim/init.vim)（用的neovim），[Jose](https://github.com/JJGO/dotfiles/blob/master/vim/.vimrc),尽量不要照搬，读读里面的文档理解后选择有用部分。

## Vim插件

Vim 有很多扩展插件。跟很多互联网上已经过时的建议相反，你不需要在 Vim 使用一个插件管理器（从 Vim 8.0 开始）。你可以使用内置的插件管理系统。只需要创建一个 `~/.vim/pack/vendor/start/` 的文件夹，然后把插件放到这里(比如通过 `git clone`),这里提供了[四个推荐插件](https://missing-semester-cn.github.io/2020/editors/#:~:text=ctrlp.vim%3A%20%E6%A8%A1%E7%B3%8A,%3A%20%E9%AD%94%E6%9C%AF%E6%93%8D%E4%BD%9C),或使用[Vim Awesome](https://vimawesome.com/)了解更多插件

## 其他应用的Vim模式

见[这里](https://missing-semester-cn.github.io/2020/editors/#:~:text=%E5%85%B6%E4%BB%96%E7%A8%8B%E5%BA%8F%E7%9A%84%20Vim,%E7%BB%91%E5%AE%9A%E7%9A%84%E8%BD%AF%E4%BB%B6%E3%80%82),vscode上也可以下载对应插件。

## Vim进阶

见[这里](https://missing-semester-cn.github.io/2020/editors/#:~:text=Vim%20%E8%BF%9B%E9%98%B6,%5D%20%E5%88%86%E9%9A%94%E7%AC%A6)

## 补充材料

- `vimtutor`输入即可进入简单教程
- [Vim adventure](https://vim-adventures.com/)，游戏化学习vim
- [Vim小技巧](https://vi.stackexchange.com/)
- [Vim Tips Wiki](http://vim.wikia.com/wiki/Vim_Tips_Wiki)
- [Vimgolf](https://www.vimgolf.com/)，做题的Code Golf
- [Practical Vim](https://pragprog.com/titles/dnvim2/),直接下载[英文版](https://zh.z-library.sk/book/5103162/5ec209/practical-vim-edit-text-at-the-speed-of-thought.html)或[中文版](https://zh.z-library.sk/book/3631943/21e438/vim%E5%AE%9E%E7%94%A8%E6%8A%80%E5%B7%A7.html)

>  **do not try to memorize, learn by usage.**

## 课后习题

第一题：很好用的tutor，可以反复做一做，很全面。<br>
其他题主要是配置方面的题，自己做做就好了。
