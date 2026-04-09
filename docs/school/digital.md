# 数字逻辑设计 | Digital Design

!!! info "主要参考"
    [Isshiki修's Notebook](https://note.isshikih.top/cour_note/D2QD_DigitalDesign/)<br>
    以及课上ppt，少量教材阅读<br>
    后文部分公式由于mkdocs的插件问题还没渲染好，以后抽空会把插件和数学公式的写法掌握一下再调整这篇里的数学公式。

??? note "中英词汇对照"
    - `radix` <=> `基数`，二进制基数为2

<hr>

## chapter 1 Digital Systems and Information

### signal

在电子信息系统中，信号分为 **模拟信号(analog signal)** 和 **数字信号(digital signal)** 两种。数字信号中又有多种表示方法，如今我们最常用的是 **二进制(binary)**。对应到电路层面，我们常常用 **高电位(HIGH)** 和 **低电位(LOW)** 来表征 `1` 和 `0`。

![数逻](https://s41.ax1x.com/2026/03/11/pek3t1A.png)

输入的判定范围比输出的大，即 **宽进严出**，可以提高电路的鲁棒性。

<hr>

### digital systems

存在两种电路:
- combinational logic system
- Sequential System(时序电路)

二者都接收离散数据并输出离散数据，主要区别在于Sequential System还存在一个 **System State**， state随时间更新，于是不同时间同样数据可能输出不同内容

### Number Systems
 
计算机领域常见的进制主要是 **二进制(binary)**，**八进制(octal)**，十进制(decimal) 和 **十六进制(hexadecimal)**。

对于一个 $r$ 进制数，它一般被写成这样：

$$
(A_{n-1}A_{n-2}...A_1A_0A_{-1}...A_{-(m-1)}A_{-m})_{r}
$$

而它对应的十进制真值为：

$$
\begin{aligned}
\sum_{i=-m}^{n-1} A_ir^i\;\;
=&A_{n-1}r^{n-1}+A_{n-2}r^{n-2}+...\\
+&A_{1}r^{1}+A_0r^0+A_{-1}r^{-1}+...\\
+&A_{-(m-1)}r^{m-1}+A_{-m}r^{-m}
\end{aligned}
$$

二，八，十六进制的转换很好理解，**三位二进制**可以表示为一位八进制数:

$$
(67.731)_{8} = (110 111.111 011 001)_{2}
$$

$$
(11 111 101.010 011 11)_{2} = (375.236)_{8}
$$

以小数点为中心，整数按右开始算对齐，小数从左边开始算对齐。而四位二进制又能表示一位十六进制数，不再赘述。而二进制与十进制:
![转化](https://s41.ax1x.com/2026/03/11/pek3Ypd.png)
若是十进制有小数转二进制，先把整数和小数分开，整数单独处理，小数部分一直乘2然后取整数部分的结果1/0，接下来又再取小数部分乘2...直到小数变成0，然后从上往下取数。

<hr>

### code

二进制编码主要分为这么几种：

- Numeric
    - 必须表达一定范围内的数字；
    - 能够支持简单且普遍的计算；
    - 和二进制数值本身有较大关联；
- Non-numeric
    - 相对灵活，因为不需要适配普遍的运算法则；
        - 灵活性指，保证编码映射关系是唯一的的情况下都可以称为合法编码；
    - 和二进制数值本身未必有关系；

#### BCD 码

##### 基本知识

首先，一位二进制数能包含的信息是 1bit，也就是一个“真”或者一个“假”。我们称一个拥有 n 个元素的二进制向量为一个 **n位二进制编码(n-bit binary code)**。一个 n 位二进制数拥有 $2^{n}$ 种可能的组合，因而可以表示 $2^n$ 种信息。

而我们需要设计的编码系统，就是将我们需要的信息映射到这 $2^n$ 个“空位”中。当然，当我们需要表示的信息数量并不是 2 的幂次时候，会出现一些 未分配(unassigned) 的比特组合。

而在这种编码中，最常用的就是 **BCD码(binary-coded demical)**。其核心思路就是，将十进制的每一位 **分别** 用 **真值相等的 4 位二进制** 表示，即 `0` ~ `9` 分别用 `0000` ~ `1001` 表示。

|Decimal<br>Symbol|BCD<br>Digit|
|:-:|:-:|
|`0`|`0000`|
|`1`|`0001`|
|`2`|`0010`|
|`3`|`0011`|
|`4`|`0100`|
|`5`|`0101`|
|`6`|`0110`|
|`7`|`0111`|
|`8`|`1000`|
|`9`|`1001`|


$$
\mathrm{ (185)_{10}\;=\;(0001\;1000\;0101)_{BCD}\;=\;(10111001)_2 }
$$

##### BCD码的运算

对同一位的数字的四位各自进行正常计算，如果最终四位的表示结果大于9，则在这一位上+6进位

![计算实例](https://s41.ax1x.com/2026/03/11/pek3GfH.png)

#### 余三码

一种 **BCD码** 的改进是 **余三码(Excess3)**。其核心思路是在 BCD码的基础上，增加一个大小为 3 的偏移量。

|Decimal<br>Symbol|Excess3<br>Digit|
|:-:|:-:|
|`0`|`0011`|
|`1`|`0100`|
|`2`|`0101`|
|`3`|`0110`|
|`4`|`0111`|
|`5`|`1000`|
|`6`|`1001`|
|`7`|`1010`|
|`8`|`1011`|
|`9`|`1100`|

为什么是 3 呢？首先这个 3 来自于 $\frac{16-10}{2}$，也就是 8421 码的容量减去我们需要表示的数字数量，再除以二。这样的好处是，十进制下能进位的两个数，在余三码下相加也刚好进位。

!!! tip "code & conversion"
    code是进行编码，而conversion是转化，概念不同！
    - conversion:<br>
        $$13_{10} = 1101_{2}$$
    - code:<br>
        13 <=> 0001 0011

#### [格雷码](https://note.isshikih.top/cour_note/D2QD_DigitalDesign/Chap01/#%E6%A0%BC%E9%9B%B7%E7%A0%81:~:text=%E6%9B%B4%E5%A4%9A%E6%98%AF%E6%95%B0%E5%80%BC%E3%80%82-,%E6%A0%BC%E9%9B%B7%E7%A0%81,-%C2%B6)<br>

理解了格雷码存在的必要，就是每次只改变一个bit，详细的变化规则:
> A Gray code for a counting sequence of n binary code words (n must be even) 
can be constructed by replacing each of the first n/2 numbers in the sequence with a 
code word consisting of 0 followed by the even parity for each bit of the binary code 
word and the bit to its left. For example, for the binary code word 0100, the Gray 
code word is 0, parity(0, 1), parity(1, 0), parity(0, 0)  =  0110. Next, take the 
sequence of numbers formed and copy it in reverse order with the leftmost 0 
replaced by a 1. This new sequence provides the Gray code words for the second n/2 
of the original n code words. For example, for BCD codes, the first five Gray code 
words are 0000, 0001, 0011, 0010, and 0110. Reversing the order of these codes and 
replacing the leftmost 0 with a 1, we obtain 1110, 1010, 1011, 1001, and 1000 for the 
last five Gray codes. For the special cases in which the original binary codes are 0 
through 2n- 1, each Gray code word may be formed directly from the correspond
ing binary code word by copying its leftmost bit and then replacing each of the 
remaining bits with the even parity of the bit of the number and the bit to its left.

!!! 自己再去理解一下啊
    所以实际的 hexadecimal Gray code就是把最底层的0~15这16个数字先转成二进制(因为最高用到第四位所以就是4位二进制)，然后在四位二进制的基础上取格雷码，再转回去。其他的比如8进制gray code就是把0~7这8个数转成3位二进制？那么如果要表示12这样的十六进制呢？
    见ai对话“格雷码” copilot篇
    十六进制格雷码表只定义了 0~F 这 16 个基本符号。

    每个十六进制符号对应一个 4 位二进制格雷码。
    所以，十六进制格雷码本质上是 逐位映射：每一位十六进制数都单独转换成格雷码。
    🔹 举例：十六进制 12
    十六进制 12 = 两位十六进制数：1 和 2。
    我们分别查表：
    十六进制 1 → 格雷码 1
    十六进制 2 → 格雷码 3
    所以十六进制数 12 的格雷码表示就是 13。
    *这个部分可以push进修的仓库*

## chapter 2 

- 对偶原则与互补函数

- Shannon formula(见ppt57)

- SOM与POM:
    - SOM就是挑出所有使值为1的组合然后or，达到 **有1出1** 的目的
    - POM则挑出所有值为0的or组合然后and，达到 **有0出0** 的目的

### 电路实现与优化

*此处按照字面量记写了两次，可以改一改*

#### 衡量标准

L,G,GN

#### 卡诺图

减少G