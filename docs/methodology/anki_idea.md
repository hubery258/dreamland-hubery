# anki基本认识与概念

!!! info 
    gpt5.6 luna进行结构化整理与图形化阐释，文本均来自本人

读了文档后基本的感觉:
> 把“知识”录入成 Note（笔记） → Anki 根据模板自动生成 Card（卡片） → Card 被放进 Deck（牌组） → Anki 根据记忆情况安排什么时候再次复习。

## 背景

间隔重复与主动学习，关键是“通过测试引发主动回想”

## 笔记、卡片、牌组

```text
Deck（牌组）
│
├── Card
├── Card
├── Card
└── Card

而 Card 来自：

Note（笔记）
│
├── Field（字段）
├── Field
└── Field
      │
      ↓
Card Template（卡片模板）
      │
      ↓
生成一个或多个 Card
```

deck本质上就是记录分类，而**Note 是录入的一条知识。** note里又可以有多个Field(字段)，最简单的笔记类型一般就是：

```text
Front:
Java 中 == 和 equals() 有什么区别？

Back:
== 对基本类型比较值……
```

这里：Front，Back就是两个 Field。

!!! example "高级设计"
    完全可以自己设计：

    ```text
    Question
    Answer
    Example
    Source
    Extra
    ```

    然后某条 Note 是：

    ```text
    Question:
    什么是 TCP 三次握手？

    Answer:
    建立 TCP 连接时双方进行三次报文交换……

    Example:
    Client -> SYN
    Server -> SYN+ACK
    Client -> ACK

    Source:
    计算机网络 Chapter 5
    ```

## Template——note变为card的方法

官方文档把 Card Type 描述成根据 Note 生成 Card 的“blueprint”；每个 Card Type 都有正面模板和背面模板。([Anki手册](https://docs.ankiweb.net/getting-started))

假设Note 有：

```text
Question
Answer
Extra
```

你可以规定：

### Front Template

```html
{{Question}}
```

### Back Template

```html
{{FrontSide}}

<hr>

{{Answer}}

<br>

{{Extra}}
```

于是输入：

```text
Question:
什么是进程？

Answer:
正在执行的程序实例。

Extra:
进程拥有独立的地址空间。
```

Anki 自动生成：

```text
        CARD

正面：
什么是进程？

背面：
什么是进程？
----------------
正在执行的程序实例。

进程拥有独立的地址空间。
```

所以：

> **Field 决定你存什么数据；Template 决定这些数据显示在哪里，如何显示；Card 是最终拿来复习的东西。**

这是整个 Anki 最核心的抽象。

---

###  一个 Note 可以生成多个 Card

比如学英语：

```text
English:
apple

Chinese:
苹果
```

你可以有两个模板。

### Card 1

```text
Front:
{{English}}

Back:
{{Chinese}}
```

生成：

```text
apple
  ↓
苹果
```

### Card 2

```text
Front:
{{Chinese}}

Back:
{{English}}
```

生成：

```text
苹果
  ↓
apple
```

于是**只维护一份数据，却可以生成两张卡。**

以后你发现：

```text
apple = 苹果
```

想加：

```text
例句：I ate an apple.
```

只需要修改 Note。

所有由这个 Note 生成的 Card 都可以自动使用新内容。

这就是为什么 Anki 不直接把所有东西都理解成“卡片”。([Anki手册](https://docs.ankiweb.net/getting-started))

---

### Note Type 

不同知识需要不同的数据结构。

!!! example 
    比如英语：

    ```text
    Note Type: Vocabulary

    Fields:
    Word
    Meaning
    Example
    Pronunciation
    ```

    CS：

    ```text
    Note Type: CS Concept

    Fields:
    Question
    Answer
    Example
    Source
    Extra
    ```

    数学公式：

    ```text
    Note Type: Formula

    Fields:
    Name
    Formula
    Explanation
    Example
    ```

所以：

> **Note Type = 一类 Note 的结构 + 它们对应的 Card Types / Templates。**

#### 自带的note type

##### Basic

```text
Front → Back
```

##### Basic (and reversed card)

自动生成：

```text
Front → Back
Back → Front
```

适合语言学习。

##### Cloze

也就是**填空卡**。

---

## Anki 的基本使用流程

日常实际上就两个阶段：

```text
阶段 A：制作材料

学习
 ↓
发现值得长期记忆的知识
 ↓
创建 Note
 ↓
Template 自动生成 Card
 ↓
进入 Deck
```

然后：

```text
阶段 B：复习

打开 Anki
 ↓
选择 Deck
 ↓
Review
 ↓
尝试回忆
 ↓
Show Answer
 ↓
Again / Hard / Good / Easy
 ↓
Anki 安排下一次复习
```

所以真正需要你维护的主要是：

**Note + Note Type + Template。**

真正每天学习的是：

**Card。**

Deck 更多只是组织这些 Card。

---

## 管理笔记和卡片

Anki Desktop 里面有一个非常重要的地方：**Browse / 浏览**, 大量管理工作都会在那里完成。

比如：

```text
搜索 Linux 相关 Note
修改 Answer
批量加 Tag
移动 Card 到另一个 Deck
查看某个 Note 生成了哪些 Card
Suspend 某些 Card
删除 Note
```

另外，Note 是**知识源数据**，Card 是它产生出来的学习项目。

所以平时最好养成一个思维习惯：

> 我是在修改“知识本身”，还是修改“这个知识应该怎么被考？”

前者通常是 Note / Field。

后者通常是 Card Type / Template。

---

## Anki ↔ AnkiDroid 同步

```text
Anki Desktop
      ↕
   AnkiWeb
      ↕
  AnkiDroid
```

AnkiWeb 是官方提供的同步服务，可以同步 collection，并同步图片、音频等媒体。([Anki手册](https://docs.ankiweb.net/syncing.html?highlight=sync))

---

## 总结

```text
Anki
│
├── Deck
│     卡片放在哪里
│
├── Note Type
│     一类知识的数据结构
│
├── Note
│     一条知识
│
├── Field
│     知识的数据字段
│
├── Card Type / Template
│     规定怎么把 Note 变成 Card
│
└── Card
      真正拿来复习的东西
```
