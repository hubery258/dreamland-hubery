# 项目逆向学习手册

!!! note "相关学习材料"
    [js菜鸟教程](https://www.runoob.com/js)  
    [react文档](https://zh-hans.react.dev/learn)

## 1. 目录结构

> 我将在学习的过程中一点一点写明每个文件的作用,当全是√的时候大概也就彻底了解了

```
dreamland
|- backend/
    |- app/ # 核心后端代码，路由、模型、业务逻辑都在这里
    |- venv/ # √ 虚拟环境，存放依赖，不需要深入。
    |- .env / .env.example # 环境变量配置（数据库连接、密钥等）
    |- blog.db # sqlite 数据库文件
    |- requirement.txt # √ 安装依赖用的
|- frontend/
    |- node_modules/ √ # 前端依赖库，不需要深入
    |- public √ # 存放静态资源，图标展示
    |- src/ # 前端核心代码
        |- api/ # api封装
            |- client.js # 调用后端接口
        |- assets/ # √ 图片、图标资源
        |- components/ # 可复用组件（卡片、标题、列表项）。
            |- FriendCard.jsx √ # 友链卡片组件
            |- Header.jsx √ # 负责控制最上方的 dreamland开始到 friends结束的head导航组件，加新页面要修改这里
            |- PageTitle.jsx √ # 每个页面的标题组件
            |- PostListItem.jsx √ # post列表的一块一块组件
            |- ProfileCard.jsx # post列表页的个人卡片组件
        |- data/ # 静态数据
            |- friends.js √
            |- profile.js √
        |- layouts/ # 页面布局
            |- MainLayout.jsx √
        |- pages/ # 页面级组件，对应不同路由（HomePage、PostDetailPage）
            |- AboutPage.jsx √
            |- FriendsPage.jsx √
            |- HomePage.jsx # 涉及后端输入
            |- NewPostPage.jsx # 涉及一点后端输入
            |- PostDetailPage.jsx # 差不多能懂，有点后端输入头痛
            |- TagsPage.jsx # 同样涉及一点后端输入，要完全懂这四个可能要等到把client.js读一下
        |- styles/ 
            |- global.css # √ 全局css文件，这个没啥好了解的
        |- App.jsx √ # 根组件，负责路由和整体结构。
        |- main.jsx √ # 前端入口文件，挂载 React 应用，会调用app.jsx，没有太大了解必要(这一条是个人感觉)
    |- .env.development / .env.example # 前端环境变量。
    |- .gitignore # √
    |- eslint.config.js
    |- index.html
    |- package-lock.json / package.json # 前端依赖与脚本。
    |- README.md # √
    |- vite.config.js # 构建工具配置
|- .gitignore # √ git操作忽略一些文件
|- record.md # √ 记录一些项目构建操作的
|- todo.md # √ 记录未来想增加的项目的
```

## 2. 前端入口

[紧急复习js](../../cs/js.md)

### JSX与React

JSX本质是一种语法糖，最终会编译到`React.createElement()`，而React极度关注**组件**，所有JSX文件中大写开头的标签均是React组件:
```jsx
function MyButton() {
  return (
    <button>我是一个按钮</button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>欢迎来到我的应用</h1>
      <MyButton />
    </div>
  );
}
```
这里我们的`<Mybutton />`就是一个React组件，其余部分和js+html差不多

#### 快速入门

1. 必须且只能有一个根元素:组件返回时只能有一个根节点，可以用 `<div>` 或 `<>...</>` 包裹，且必须**闭合标签**(`<br/>`)
    ```jsx
    return (
        <div>
            <h1>标题</h1>
            <p>内容<br/>内容</p>
        </div>
    )
    ```
2. 属性类似html，但用驼峰命名:
    ```jsx
    <button onClick={handleClick}>点我</button>
    <input type="text" value={name} />
    ```
3. 插值表达：用`{}`在标签里插入js变量/表达式:
    ```jsx
    const name = "fk"
    return <h1>Hello, {name}!</h1>
    ```
4. 条件渲染：可以直接用js的if，else，也可以用**三元运算符或逻辑运算符**:
    ```jsx
    // 等价
    let content;
    if (isLoggedIn) {
        content = <AdminPanel />;
    } else {
      content = <LoginForm />;
    }
    return (
        <div>
            {content}
        </div>
    );

    <div>
      {isLoggedIn ? (
        <AdminPanel />
      ) : (
        <LoginForm />
      )}
    </div>

    // 不需要else分支时
    <div>
      {isLoggedIn && <AdminPanel />}
    </div>
    ```
5. 声明事件处理函数来响应事件:
    ```jsx
    function MyButton() {
      function handleClick() {
        alert('You clicked me!');
      }

      return (
      <button onClick={handleClick}>
        点我
      </button>
      );
    }
    ```
    注意不是调用事件处理函数，只是把**函数传递给事件**
6. 通过`import { useState } from 'react'`引入useState，可以实现组件更新:
    ```jsx
    function MyButton() {
        /* 得到两个数据， state(count), 以及用于更新它的函数
        这里实现了点击一次更新点击次数，初始state为设定的0*/
        const [count, setCount] = useState(0); 

        function handleClick() {
            setCount(count + 1);
        }

        return (
        <button onClick={handleClick}>
            Clicked {count} times
        </button>
        );
    }
    ```
7. [组件间共享数据](https://zh-hans.react.dev/learn#sharing-data-between-components)

#### 首先搞明白了: `app.jsx`、`page/`以及`components/`、`layouts/`的大部分

- `app.jsx`: 在Routes组件下我们把初始path定为/然后基础渲染是HomePage组件其余就是我们对应的path长什么样我们就用Route组件渲染被选中的element组件而已,**所以如果以后还要加页面，就是在这里加路由**
    - 特别地：`:slug` → 动态路由参数，占位符，我们可以用`useParams()` → 在组件里获取实际的值 
- `components/`下面就存放了各种可复用组件，我们需要新加一些模块而非页面时（*比如增加评论区的一条一条评论样式结构，或者一个新的音乐卡片之类的*）就要在该目录下新建组件，然后在对应的需要加的pgae文件上调用这个组件，传入数据即可
    - 调用率比较高的是`Header.jsx` :负责控制最上方的 dreamland开始到 friends结束的head导航组件，**加新页面要在这里加入对应路由以及显示**
- `page/` 直接存放页面，我们的page实际上就是通过组件拼出来的
- `layouts/`下的`MainLayout.jsx`决定着页面整体的布局，不难理解

##### 阶段性总结

1. 基本搞定了React的纯前端内容，涉及到与后端数据交互以及`client.js`可能还有点头痛
2. 到目前为止我认知里的web应用工作流：
    - 后端(比较黑箱，简单从以前的项目推推)
        - 后端（比如 FastAPI、Flask）从数据库里取出原始数据，可能做一些简单处理（过滤、排序、拼接）
        - 后端把数据通过接口暴露出来。  
            - 例如：`GET /posts/:slug` 返回某篇文章的数据。
        - 前端通过 `fetch` 或 `axios` 发出请求。  
        - 后端返回 JSON 格式的数据。

    - 前端
        - “一开始的创造我不是很懂”  
        - 在 `App.jsx`或者其他语言的类似地位文件(flask里的`app.py`)里定义路由规则。  
            - 浏览器访问某个 URL 时，Router 匹配对应的页面组件/文件。  
        - 渲染对应的页面构建文件夹里的组件/文件。(React来说就是`page/`的组件被渲染)  
            - (React而言)页面内部会调用 `components/` 下的复用组件。
        - 页面组件通过调用 `client.js`（封装好的 API 请求）**获取后端数据**。（flask里好像也是`app.py`做的）  
        - (react而言)拿到数据后用 `useState` / `useEffect` 更新组件状态，**触发重新渲染**，最终在浏览器里展示最新内容。
        - 用户填写表单或点击按钮。  
            - 前端收集数据并通过 API 请求传回后端。  
        - 后端更新数据库，再返回新的结果，前端根据返回结果更新界面。循环

