# anki基本概念与介绍

> gpt5.6 luna进行结构化整理与图形化阐释，文本均来自本人  
> [官方介绍文档与下载指南](https://open-spaced-repetition.github.io/anki-manual-zh-CN)

## Anki 是什么？
Anki 是一款辅助记忆与复习的软件。它主要建立在两个很简单的想法上：**主动回忆和间隔重复**。  
最基础的使用方式，可以把它理解成《How to Learn》中提到的“闪卡（Flashcard）”：卡片正面写一个问题，背面写答案。  
复习的时候，先看正面的问题，尽量自己回忆答案，再翻到背面检查。相比于反复阅读笔记，这个过程要求自己主动把知识“想起来” 。
  
而 Anki 在闪卡的基础上又做了一件事：**帮你决定什么时候应该再次看到这张卡片。**  
一张总是答错的卡片会更频繁地出现；一张已经很熟悉的卡片，则会隔更长时间再出现。  
这就是 Anki 最基本的用途：把需要长期记忆的东西做成可以不断测试自己的卡片，再由软件安排之后的复习。
  
不过真正开始使用 Anki 后，会发现它并不只是简单的“正面写问题、背面写答案”。里面还有 Note、Card、Deck、Field、Template 等一系列概念。   
我在读了一遍官方文档后，试着把这些概念和 Anki 的基本工作方式整理了下来。

![anki总览](https://pic2.zhimg.com/v2-6adae0ff4fb8f3a0c56e8034463308ef_1440w.jpg)

基本的脉络：
> 把“知识”录入成 Note（笔记） → Anki 根据模板自动生成 Card（卡片） → Card 被放进 Deck（牌组） → Anki 根据记忆情况安排什么时候再次复习。


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
Front
Back
```

但完全可以自己设计：

![配图1](https://picx.zhimg.com/v2-c7941bdff088ecaa4ad51c0f6c921545_1440w.jpg)

这里的英语单词 Note 的字段界面右侧就有大量字段(field)：英语单词、音标、中文释义、英/中例句、Vocabulary 简明/扩展等

## Template——note变为card的方法

官方文档把 Card Type 描述成根据 Note 生成 Card 的“blueprint”；每个 Card Type 都有正面模板和背面模板。([Anki手册](https://docs.ankiweb.net/getting-started))

仍然以英语单词为例,英语单词正面 Template + 预览。实际上模板是支持html编辑的：
![英语正面](https://pic2.zhimg.com/v2-b4b7562706e9a80fda922961da5a45f9_1440w.jpg)

而翻到背面以后，则可以把刚才 Note 中保存的其他 Field 全部调出来：
![英语背面](https://pic4.zhimg.com/v2-a54b6e3617eb79f0b5ad2cdfbfcbee15_1440w.jpg)

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

#### Card 1

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

#### Card 2

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

不同知识需要不同的数据结构。比如刚才的英语单词，需要音标、释义、例句等字段。

但我还做了另一种完全不同的 Note Type，用来记录平时遇到的句子：

![句子](https://pic2.zhimg.com/v2-8395c344cfa9ef709653b9f3401a86af_1440w.jpg)

它的目的不是“记住一个单词是什么意思”，而是希望以后看到提示时，能够主动回忆自己读过的某句话。

因此我给它设计的正反面是:

![句子card正面](https://picx.zhimg.com/v2-094d71d81e1b472e66ac7fafcfcb471f_1440w.jpg)

![句子card背面](https://pic2.zhimg.com/v2-42503bc058c686a77090da6de36e4a7b_1440w.jpg)

可以看到，它和英语单词用的是**完全不同的数据结构和完全不同的测试方式。**

这就是 Note Type 存在的意义。

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

anki最强大的地方在于极高的定制化适配，允许你以不同的模板对待不同的note内容，且flash card式的设计让它实现了**主动回忆与间隔重复**，这恰恰是记忆知识最需要的东西，期待大家读完本篇后能试着使用anki，爱上anki！

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
