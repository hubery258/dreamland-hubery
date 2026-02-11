# cs61a: Data

## 原生数据类型

Python中每个值都有一个**类**(class)决定值得数据类型。属于一个类，可供选择的行为模式也相同，有三种原生**数值**类型:`int`,`float`,`complex`(复数):
```python
>>> type(1+1j)
<class 'complex'>
```
`int`没有边界，但`float`值在很多情况下是实数的近似值，存在误差，一个`int`/另一个`int`会产生`float`值，如`1/3`。

## 数据抽象

数据抽象将复合数据值的使用方式与其构造细节隔离开来。数据抽象的基本理念是构造程序，使其操作抽象数据。也就是说，程序在使用数据时，应尽可能少地对数据做假设。同时，具体的数据表示被定义为程序中独立的一部分。

???+ abstract "数据抽象的核心"
    **处理数据表示的部分**与**处理数据操作的部分**隔离开来的通用技术，是一种强大的设计方法论，他不关心内部实现。(见[city](https://www.learncs.site/docs/curriculum-resource/cs61a/cs61a_zh/lab/lab04#%E5%9F%8E%E5%B8%82))称为**数据抽象**（data abstraction）:
    1. 有一个**构造函数负责选择内部数据结构，把数据封装起来成为一个对象**
    2. 有**选择器**，从对象中取出需要信息，屏蔽内部结构细节
    3. **外部代码不关心对象内部是 dict、list、tuple，只关心构造函数和选择器。**如果内部结构改变只需要修改构造函数和选择器<br>
    总结而论: 数据发给构造函数，构造函数按需要将数据以某种数据结构封装成对象，选择器只需访问封装对象，**不用在输出数据的第一时间考虑形式，只需要交给构造函数调整即可，而要修改也只需要改变构造函数的封装形式**，其他地方不用调整,具体表现可以见project中`cat`的problem 9.

关于[抽象屏障](https://composingprograms.netlify.app/2/2#_2-2-3-%E6%8A%BD%E8%B1%A1%E5%B1%8F%E9%9A%9C),我的语言就是程序的不同部分用不同方式对待数据，有理数计算就直接把有理数当x对待就够了，涉及有理数的构造会出现分数，此时用x/y才够用，这是而如果在高层级的有理数计算中用了x/y的数据形式，就出现了**抽象屏障违法**

## 序列

**序列**(sequence)是**有序**的值的集合,序列有很多种，但都拥有共同的行为,特别是:
- 长度: 长度有限，空序列长度为0
- 元素选择: 序列拥有与索引相对应的元素，从0开始表示第一个元素

下面介绍两种内置属于序列的数据类型:

### 列表(list)

列表类似于数组，**对序列而言加法和乘法是对序列本身进行组合与复制**.负索引从末尾开始计数，最右边元素的索引是`-1`

当涉及到序列遍历时，常用到`for`语句:
```python
for <name> in <expression>:
    <suite>
```
`<expression>`中每个元素会依次遍历，在`for`执行完后`<name>`绑定到了序列的最后一个元素，且`for`语句头部可包含多个名称:
```python
>>> pairs = [[1, 2], [2, 2], [2, 3], [4, 4]]
>>> same_count = 0
>>> for x, y in pairs:
        if x == y:
            same_count = same_count + 1

>>> same_count
2
```
多个值绑到多个名称上的模式为**序列解包**，如果语句中用不到`<name>`可以直接`_`

### 范围(ranges)

```python
>>> range(1, 10)  # [1, 10)，包括 1，但不包括 10
range(1, 10)
>>> list(range(5, 8))
[5, 6, 7]
>>> list(range(4))
[0, 1, 2, 3] # 只给一个参数从0开始
```

### 元组(tuple)

将不同数据用逗号分隔，即为元组,与列表不同，元组本身不可变，但如果其内部的元素本身是可变数据即可操作
```python
>>> 1, 2 + 3
(1, 5)
>>> ("the", 1, ("and", "only"))
('the', 1, ('and', 'only'))
```

### 序列处理

#### 列表推导式

用于对序列中的每个元素求值一个固定表达式，并将结果值收集到一个结果序列中,**尤其在从一个列表中新建一个列表时非常好用(cats problem 9)**:
```python
>>> odds = [1, 3, 5, 7, 9]
>>> [x+1 for x in odds if 25 % x == 0]
[2, 6]
```
一般格式:`[<映射表达式> for <名称> in <序列表达式> if <过滤表达式>]`,首先求值`<序列表达式>`,返回可迭代值，再按顺序处理，元素绑定后求值过滤表达式，结果为真就求映射表达式再收集到列表中

构建空列表，再在空列表里放很多空列表作为初始状态:`result = [ [] for _ in player_indices]`

#### 聚合(Aggregation)

将序列中的所有值聚合为单个值，内置函数 `sum`、`min` 和 `max` 都是聚合函数的例子。

#### 成员资格(Membership)与切片(Slicing)

```python
>>> digits
[1, 8, 2, 8]
>>> 2 in digits
True
>>> 1828 not in digits
True
```

[切片](http://getpython3.com/diveintopython3/native-datatypes.html#slicinglists): 第一个整数表示切片起始索引，第二个表示结束索引的**后一位**:
``` python
>>> digits[0:2]
[1, 8]
>>> digits[1:]
[8, 2, 8]
```

### 字符串(string)

**字符串字面量**（string literals）可以表示任意文本，由单引号或双引号包围,要使用多行字符串时三引号。

成员资格匹配时:
```python
>>> 'here' in "Where's Waldo?"
True
```

`\n`在计算多行字符串长度与元素选择时被视为单个字符

用`str()`将对象值强制转换为字符串形式
```python
>>> str(2) + ' is an element of ' + str(digits)
'2 is an element of [1, 8, 2, 8]'
```

### 树(tree)

树具有一个**根标签**（root label）和一个**分支**（branches）序列,**树的每个分支本身也是一棵树**。**没有分支的树称为叶子（leaf）**。包含在树内部的任何树都称为该树的子树（sub-tree，例如分支的分支）。每棵子树的根称为该树中的一个**节点**(node), 只有当**一棵树具有根标签且所有分支也都是树**时，它才是结构良好（well-formed）的。

!!! abstract '树的关键'
    是树的关键就是:
    - 是一个列表
    - 有一个根标签
    - 分支也要符合上述两个条件/没有分支

[还没看](https://composingprograms.netlify.app/2/3#_2-3-6-%E6%A0%91)

### 链表

## 对象(object)

对象将信息与函数/操作结合在了一起，Python中所有**值**都是对象，都有**行为和属性**，行为偏向于值其运算与内在处理，而属性是其内在的设定。

??? info '类'
    对象的行为是通过特定语法与术语实现的:
    ```python
    >>> from datetime import date
    ```
    `date`就是一个**类**(class)，代表一种值的类型(个人理解:定义出特征及可能运算)，具体日期称为该类的**实例**(instances),可通过调用类并传入描述实例的参数来构造实例。

对象有**属性**(attributes),可理解为对象中某个值的名字，用`.`访问；对象还有**方法**(methods),其实也是一种属性，但是属性的值是**函数**，方法根据他们的参数与所属对象来计算结果。

有的变量绑到一个对象上，对某个变量做操作同样影响另一个变量(对象一致)，有时两个变量内的列表元素值相同，仍可能是完全不同的两个列表对象，可以用`is`与`is not`两种比较操作符验证,[例](https://composingprograms.netlify.app/2/4#_2-4-2-%E5%BA%8F%E5%88%97%E5%AF%B9%E8%B1%A1):
```python
>>> suits is nest[0]
True
>>> suits is ['heart', 'diamond', 'spade', 'club']
False
>>> suits == ['heart', 'diamond', 'spade', 'club']
True
```
**一个判断内存地址，一个判断内容是否一致**

## 字典

字典由键值对构成，可以通过key查找对应的value，key必须唯一。

可以用`.keys()`,`.values()`或`.items()`访问键，值或键值对，`in`可检查字典是否含某个key，字典推导式可以快速创建字典
```python
>>> {3*x: 3*x + 1 for x in range(2, 5)}
{6: 7, 9: 10, 12: 13}
```
字典的key不可以是可变数据,一个方法是`get`,返回指定key对应value，若key不存在，则返回默认值:
```
>>> numerals = {'V': 5}
>>> numerals.get('A': 0)
0
>>> numerals.get('V': 0)
5
```

列表和字典拥有局部状态（local state），即它们可以在程序执行过程中的某个时间点修改自身的值。状态（state）就意味着当前的值有可能发生变化。函数也有状态:有时同一个函数接收相同值返回的值不同，就不是纯函数(in-pure)，有副作用(side effects),如[取钱输出剩余存款](https://composingprograms.netlify.app/2/4#_2-4-4-%E5%B1%80%E9%83%A8%E7%8A%B6%E6%80%81),需要`nonlocal`声明让变量成为某种c语言意义上的**静态变量**，但不同调用会生成不同的变量，比如
```python
def make_withdraw(balance):
	def withdraw(amount):
	    nonlocal balance
	    if amount > balance:
	        return 'Insufficient funds'
	    balance = balance - amount
	    return balance
	return withdraw
	
wd = make_withdraw(20)
wd2 = make_withdraw(7)
wd2(6)
wd(8) # 2里balance变化不影响1
```
只是初始赋值要在函数之外。非局部语句（nonlocal statement）会改变所在函数定义中剩余的所有赋值语句。在将声明为 `nonlocal`的变量尝试赋值的语句中都不会直接在当前帧中寻找并更改值，而是找到定义变量的帧，并在该帧中更新该变量。如果在声明 `nonlocal` 之前还没有赋值，则 `nonlocal` 声明将会报错。

## 约束传递

[还没看](https://composingprograms.netlify.app/2/4#_2-4-9-%E7%BA%A6%E6%9D%9F%E4%BC%A0%E9%80%92-propagating-constraints)