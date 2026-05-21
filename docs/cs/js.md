# JavaScript

> 主要学习进度到[菜鸟教程](https://www.runoob.com/js)的`js运算符`，因为主要是为了学react，所以掌握下面基础知识就先够了，有空再学

脚本语言，用于发出命令与获取数据，网页交互操作，大小写敏感，会忽略多余空格。

## 输出

- 使用 `window.alert()` 弹出警告框。
- 使用 `document.write()` 方法将内容写到 HTML 文档中。
- 使用 `innerHTML = ...` 写入到 HTML 元素。
    - 访问HTML元素：`document.getElementById(id)`，通过id访问HTML元素，然后再`.innerHTML`来获取或插入元素内容
- 使用 `console.log()` 写入到浏览器的控制台（f12后在console中找到对应写入）。

## 变量与字面量

1. 定义对象: `{firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"}`
2. 定义函数：`function myFunction(a, b) { return a * b;}`
3. 定义变量: 
    - `var`来定义，等号赋值: `var x; x = 4;`， **有函数作用域**
        - `var`可以重复声明，变量未赋值时默认值为undefined
        - 可以一行声明多个，声明也可以横跨多行，`,`分隔，但不能多个变量一起赋值`var x,y,z = 1;`
    - `const`， **块级作用域，值不可变**
    - `let`, **块级作用域**
    - 以字母开头，或者`$`/`_`(*正常人都不会用后面两个吧*)

!!! note inline end "let与var"
    作用范围区别
    ```javascript
    function varTest() {
        var x = 1;
        if (true) {
            var x = 2;       // 同样的变量!
            console.log(x);  // 2
        }
    console.log(x);  // 2
    }

    function letTest() {
        let x = 1;
        if (true) {
            let x = 2;       // 不同的变量    
            console.log(x);  // 2  
        }
        console.log(x);  // 1
    }

## 对象

对象是**拥有属性与方法的数据**， 在js中由花括号分隔。在括号内部，对象的属性以名称和值对的形式 **(name : value)** 来定义。属性由逗号分隔：`var person={firstname:"John", lastname:"Doe", id:5566};`

对象有两种寻址方式:
```javascript
name=person.lastname;
name=person["lastname"];
```

!!! note
    在js中几乎所有事物都是对象,**JavaScript 对象是属性和方法的容器。**

    for example:
    ```javascript
    var person = {
        firstName: "John",
        lastName : "Doe",
        id : 5566,
        fullName : function() 
	    {
            return this.firstName + " " + this.lastName;
        }
    };
    document.getElementById("demo").innerHTML = person.fullName(); // 输出" John  Doe"
    document.getElementById("demo1").innerHTML = person.fullName // 输出 "function() { return this.firstName + " " + this.lastName; }"
    ```

### 数组方法(React常用)

1. **map**: 对数组每个元素执行函数并返回新数组
    ```javascript
    const nums = [1, 2, 3]
    const squares = nums.map(x => x * x)
    console.log(squares) // [1, 4, 9]
    ```
    React中我们可用来渲染列表:
    ```javascript
    const posts = ['Post1', 'Post2']
    return (
        <ul>
            {posts.map(p => <li key={p}>{p}</li>)}
        </ul>
    )

    ```

2. **filter**: 筛选符合条件的元素
    ```javascript
    const nums = [1, 2, 3, 4]
    const evens = nums.filter(x => x % 2 === 0)
    console.log(evens) // [2, 4]
    // === 是完全等于，数据类型和数值要都一样
    ```

3. **reduce**: 累计计算，返回一个值
    ```javascript
    const nums = [1, 2, 3, 4]
    const sum = nums.reduce((acc, cur) => acc + cur, 0)
    console.log(sum) // 10
    ```

4. **forEach**:遍历数组，不返回新数组（*感觉适合用来嵌套*）
    ```javascript
    const nums = [1, 2, 3]
    nums.forEach(x => console.log(x))
    ```

5. **find**: 找到第一个符合条件的元素
    ```javascript
    const nums = [1, 2, 3, 4]
    const firstEven = nums.find(x => x % 2 === 0)
    console.log(firstEven) // 2
    ```

## 函数

1. 只希望退出函数时可以用`return` 不跟返回值
2. 变量在函数内没有声明`name = "ja"` 则这个变量变为全局变量，属于 **window对象**，可以通过`window.name`在外部调用 

### 箭头函数

*感觉有点类似于lamba function的思想*
```javascript
// 普通函数
function add(a, b) {
  return a + b
}

// 箭头函数
const add = (a, b) => a + b
// const name = 参数 => 操作
```

没有自己的this:
```javascript
// 普通函数
function show() {
  console.log(this) // 在不同调用方式下，this可能是window或对象
}

// 箭头函数
const show = () => {
  console.log(this) // 始终继承外层作用域的this
}
```

让代码更简洁，避免this的困扰

## 事件

HTML事件即是HTML元素上的事，可能是浏览器行为，也可能是用户行为，在事件触发时 JavaScript 可以执行一些代码。

HTML 元素中可以添加事件属性，使用 JavaScript 代码来添加 HTML 元素:`<some-HTML-element some-event="JavaScript 代码">`

```html
<button onclick="getElementById('demo').innerHTML=Date()">现在的时间是?</button>
<button onclick="this.innerHTML=Date()">现在的时间是?</button>
```
**this**就是指代自身，而`onclick`属性就是点击事件

### HTML DOM事件

## 模块导入与导出

### 导出

1. 命名导出: 可导出多个函数/变量，但导入时必须名字相同:
    ```javascript
    // mathUtils.js
    export const add = (a, b) => a + b
    export const subtract = (a, b) => a - b
    ```
    ```javascript
    // main.js
    import { add, subtract } from './mathUtils.js'
    console.log(add(2, 3)) // 5
    ```
2. 默认导出: 每个文件只能有一个默认导出，导入时名字任取:
    ```javascript
    // logger.js
    export default function log(msg) {
    console.log(`[LOG]: ${msg}`)
    }
    ```
    ```javascript
    // main.js
    import logs from './logger.js'
    log('Hello World') // [LOG]: Hello World
    ```
    有固定名字的导入时`{}`不可缺少

## 异步与Promise

JavaScript 是**单线程**的：一次只能做一件事。如果遇到耗时操作（比如网络请求、读文件），不能阻塞整个程序，而异步编程让 JS 可以 先发出请求，继续执行其他代码，等结果回来再处理

Promise就是承诺未来会返回结果,三种状态:
- pending: 进行中
- fulfilled: 成功，返回结果
- rejected: 失败，返回错误

```javascript
const promise = new Promise((resolve, reject) => {
  let success = true
  if (success) {
    resolve("成功结果")
  } else {
    reject("失败原因")
  }
})

promise
  .then(result => console.log(result))   // 成功时执行
  .catch(error => console.log(error))    // 失败时执行

```

!!! example "更具体的例子"
    ```javascript
    function fakeFetch() {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                const data = { user: "Jun", blog: "Dreamland" }
                resolve(data)   // 模拟成功返回数据
            }, 2000)          // 2秒后返回
        })
    }

    fakeFetch()
        .then(data => console.log("拿到数据:", data))
        .catch(err => console.error("出错:", err))
    ```
    程序阅读到promise后会执行其他代码，两秒后打印`.then`这一组的东西

promise还有一种写法:
```javascript
async function getData() {
  try {
    const response = await fetch('https://api.example.com/data')
    const data = await response.json()
    console.log(data)
  } catch (error) {
    console.error(error)
  }
}
```

更易读
## tips

1. 用`\`换行:
```javascript
document.write("你好 \ 
世界!");
```

2. JavaScript作为脚本语言，浏览器在读取代码时是逐行执行脚本代码，不会提前编译
2. 可以使用内置属性`length`计算字符串长度:
    ```javascript
    var txt = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    var sln = txt.length;
    ```
    需要注意原始值字符串本身没有属性和方法，变量是对象才具有方法和属性，所以`"ABCD...".length`是错误的
3. 模板字符串： 在字符串中嵌入表达式和变量,语法：
    ```javascript
    `string text`

    `string text line 1
    string text line 2`

    `string text ${expression} string text`

    tagFunction`string text ${expression} string text`
    ```
    - **string text**：将成为模板字面量的一部分的字符串文本。几乎允许所有字符，包括换行符和其他空白字符。
    - **expression**：要插入当前位置的表达式，其值被转换为字符串或传递给 tagFunction。
    - **tagFunction**：如果指定，将使用模板字符串数组和替换表达式调用它，返回值将成为模板字面量的值。
    - 作用在于：
        ```javascript
        let text = `He's often called "Runoob"`;
        ```
        如果要用`''`或者`""`去替换，都会出现区分不了的情况，以及
        ```javascript
        const multiLineText = `
        This is
        a multi-line
        text.
        `;
        ```
        不再需要自己写转义字符\\即可分行写一行的句子
    - 占位符:`${...}`,我们可以在字符串中加入变量:
        ```javascript
        const name = 'Runoob';
        const age = 30;
        const message = `My name is ${name} and I'm ${age} years old.`;
        ```
        会在运行时求值
        ```javascript
        let header = "";
        let tags = ["RUNOOB", "GOOGLE", "TAOBAO"];

        let html = `<h2>${header}</h2><ul>`;
        for (const x of tags) {
            html += `<li>${x}</li>`;
        }

        html += `</ul>`;
        ```
        就有列无序列表的效果