# markdown note 🖊

1. **headliine/ 标题**
    - 用 `#` 来表示标题， `#` 越多表示标题大小越小
    - *别的方法就别掌握了，容易不互通*
    - tips： 一般来说这些符号都得跟一个空格再输入文本，除开强调和斜体。
1. **paragraph / 段落基本写作**
    - 不要用tab或space直接缩进段落，可能产生奇怪的排版效果
    - 直接写就好了，忍住，不要去空格
    - 对于换行有两种语法：
        1. （推荐）使用HTML的`<br>`标签跟在句子最末尾后直接回车
        2. 输入两个空格后回车，但是这个容易看不到，其实也还行
    - 如果有需要强调的文字： `**强调的文字**` <br>
       如果是需要斜杠的文字： `*斜杠文字*`  <br>
       两个都需要： `***文字***` <br>
        ***虽然下划线其实也可以做到一样效果，但是基于美观和适配问题
        （即当只标红词汇中某个部分时不能下划线）建议只学一种语法***
1. **分割线**
    - 可以使用`***(或更多*)` , `---（或更多）` , `________` 三种方式创建分割线
    - 注意分割线上留一行空白，下方为了兼容最好也留 
1. **引用块**
    - 使用`>` 可以加载出引用块，在`>`基础上`>>`可以二级引用，示例：
    > 一级引用
    > 继续保持
    >> 二级

```
- **Bold**
- _Italics_
- ~~Strikethrough~~
- <ins>Underline</ins>
- <sup>Superscript</sup>
- <sub>Subscript</sub>
- Abbreviation: <abbr title="HyperText Markup Language">HTML</abbr>
- Citation: <cite>&mdash; Chester How</cite>
```

```
$$
(A_{n-1}A_{n-2}...A_1A_0A_{-1}...A_{-(m-1)}A_{-m})_{r}
$$ // 下缀以及一些新鲜的语法

$$
\begin{aligned}
\sum_{i=-m}^{n-1} A_ir^i\;\;
=&A_{n-1}r^{n-1}+A_{n-2}r^{n-2}+...\\
+&A_{1}r^{1}+A_0r^0+A_{-1}r^{-1}+...\\
+&A_{-(m-1)}r^{m-1}+A_{-m}r^{-m}
\end{aligned}
$$
```
$$
(A_{n-1}A_{n-2}...A_1A_0A_{-1}...A_{-(m-1)}A_{-m})_{r}
$$

$$
\begin{aligned}
\sum_{i=-m}^{n-1} A_ir^i\;\;
=&A_{n-1}r^{n-1}+A_{n-2}r^{n-2}+...\\
+&A_{1}r^{1}+A_0r^0+A_{-1}r^{-1}+...\\
+&A_{-(m-1)}r^{m-1}+A_{-m}r^{-m}
\end{aligned}
$$

`&emsp;`表示空格(一个汉字宽度)

1. 列表(记得整合讲缩进)
1. 链接与图片
1. 转义字符


学习资源：[markdown语法](https://markdown.com.cn/basic-syntax/ "通用语法解析")<br>
[markdown语法for simplenote](https://simplenote.com/help/#markdown "也就是我在用的软件")