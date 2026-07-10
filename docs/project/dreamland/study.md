# 项目逆向学习手册

!!! note "相关学习材料"
    [js菜鸟教程](https://www.runoob.com/js)  
    [react文档](https://zh-hans.react.dev/learn)
    [菜鸟教程网站建设指南](https://www.runoob.com/web/web-buildingprimer.html)

## 1. 目录结构

> 我将在学习的过程中一点一点写明每个文件的作用,当全是√的时候大概也就彻底了解了

```
dreamland
|- backend/
    |- app/ # 核心后端代码
        |- main.py √ # FastAPI 入口：创建实例、注册路由、配置 CORS、自动建表
        |- database.py √ # 数据库连接配置（SQLAlchemy engine + session）
        |- models.py √ # ORM 表结构定义（Post、Tag、SiteMeta 三张表 + 多对多中间表）
        |- schemas.py √ # Pydantic 请求/响应模型，前后端之间的"数据合同"
        |- crud.py √ # 数据库增删改查逻辑
        |- routers/
            |- posts.py √ # 文章相关 API 接口
            |- tags.py √ # 标签相关 API 接口
            |- site.py √ # 站点信息接口（About 页）
    |- venv/ # √ 虚拟环境，存放依赖，不需要深入。
    |- .env / .env.example # √ 环境变量配置（ADMIN_SECRET、CORS_ORIGINS）
    |- blog.db # √ SQLite 数据库文件
    |- requirement.txt # √ 安装依赖用的
|- frontend/
    |- node_modules/ √ # 前端依赖库，不需要深入
    |- public √ # 存放静态资源，图标展示
    |- src/ # 前端核心代码
        |- api/ # api封装
            |- client.js √ # 调用后端接口
        |- assets/ # √ 图片、图标资源
        |- components/ # 可复用组件（卡片、标题、列表项）。
            |- FriendCard.jsx √ # 友链卡片组件
            |- Header.jsx √ # 负责控制最上方的 dreamland开始到 friends结束的head导航组件，加新页面要修改这里
            |- PageTitle.jsx √ # 每个页面的标题组件
            |- PostListItem.jsx √ # post列表的一块一块组件
            |- ProfileCard.jsx  √ # post列表页的个人卡片组件
        |- data/ # 静态数据
            |- friends.js √
            |- profile.js √
        |- layouts/ # 页面布局
            |- MainLayout.jsx √
        |- pages/ # 页面级组件，对应不同路由（HomePage、PostDetailPage）
            |- AboutPage.jsx √
            |- FriendsPage.jsx √
            |- HomePage.jsx √ # 原理和postdetail没有太大区别
            |- NewPostPage.jsx √ # 类似
            |- PostDetailPage.jsx √ # 通过client.js读到后端数据，然后通过state渲染 
            |- TagsPage.jsx √ # 类似
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

#### React组件逻辑:state与props的数据流

##### state（组件内部状态）

- **定义**：组件内部可变的数据，用 `useState` 管理。
- **特点**：
  - 只能在组件内部使用。
  - 更新 state 会触发组件重新渲染。
- **示例**：
  ```jsx
  import { useState } from "react";

  function Counter() {
    const [count, setCount] = useState(0);

    return (
      <button onClick={() => setCount(count + 1)}>
        点击了 {count} 次
      </button>
    );
  }
  ```

每次点击按钮，`count` 更新，组件重新渲染。(实际返回了一个数组，state值负责记录，另一个是更新函数，用于更新state)

#### props（组件间数据传递）
- **定义**：父组件传递给子组件的数据。
- **特点**：
  - 单向数据流：父 → 子。
  - 子组件不能直接修改 props，只能读取。
- **示例**：
  ```jsx
  function Greeting({ name }) {
    return <h1>Hello, {name}!</h1>;
  }

  function App() {
    return <Greeting name="Jun" />;
  }
  ```
  `App` 把 `"Jun"` 作为 props 传给 `Greeting`，子组件渲染时使用。

#### state 与 props 的配合
- 父组件管理数据（state），通过 props 分发给子组件。
- 子组件通过事件回调把用户操作传回父组件。

- 示例：一个简单的评论输入框
```jsx
function Comment({ text }) {
  return <p>{text}</p>;
}

function CommentBox() {
  const [comment, setComment] = useState("");

  return (
    <div>
      <input
        value={comment}
        onChange={(e) => setComment(e.target.value)}
      />
      <Comment text={comment} />
    </div>
  );
}
```
`CommentBox` 管理 state，`Comment` 通过 props 接收数据并展示。

#### useEffect（副作用）
- **作用**：处理异步逻辑或生命周期事件，比如 API 请求。
- **示例**：
  ```jsx
  import { useEffect, useState } from "react";

  function PostList() {
    const [posts, setPosts] = useState([]);

    useEffect(() => {
      fetch("/api/posts")
        .then(res => res.json())
        .then(data => setPosts(data));
    }, []); // [] 表示只在组件挂载时执行一次

    return (
      <ul>
        {posts.map(p => <li key={p.id}>{p.title}</li>)}
      </ul>
    );
  }
  ```
  组件挂载时请求后端数据，拿到结果后更新 state，触发渲染。此处逻辑js里有类似。

#### 📊 总结

| 概念 | 定义 | 特点 | 示例场景 |
|------|------|------|----------|
| **state** | 组件内部状态 | 可变，触发渲染 | 计数器、表单输入 |
| **props** | 父传子数据 | 单向流动，只读 | 页面标题、卡片内容 |
| **useEffect** | 副作用钩子 | 异步逻辑、生命周期 | API 请求、订阅 |

### 前端的`client.js`文件解析

一开始我们设置后端API的根地址,如果没有在`.env`里设置`VITE_API_BASE_URL`就会默认用本地FastAPI(`http://127.0.0.1:8000`)

最重要的是下面这一块通用请求函数:
```javascript
async function request(path, options = {}) {
  const response = await fetch(`${BASE_URL}${path}`, {
    ...options,
    headers: {
      "Content-Type": "application/json",
      ...(options.headers || {}),
    },
  });

  if (!response.ok) {
    const errorText = await response.text();
    throw new Error(errorText || "请求失败");
  }

  return response.json();
}

```

1. 在这个函数中，我们首先拼接好完整url并且fetch:`${BASE_URL}${path}`
2. 合并请求参数`options`(如果有)，并确保 `Content-Type` 是 `JSON`。
3. 如果返回失败就抛出错误
4. 如果成功就返回`JSON`数据

我们通过`client.js`集中了API，逻辑集中管理，不再需要在每个页面里重复写`fetch`

!!! note "JSON"
    虽然在过去的学习中无数次碰到JSON，也算在没学定义之前就大概搞懂了什么是JSON，不过还是系统地学一学吧  
    JSON是一种交换格式，以**键值对**构成，key必须用双引号包裹，值如果是字符串也必须**用双引号**，belike:  
    ```json
    {
        "title": "我的第一篇文章",
        "author": "Jun",
        "views": 120,
        "tags": ["React", "前端", "学习"],
        "published": true,
        "comments": [
          { "user": "Alice", "text": "写得很好！" },
          { "user": "Bob", "text": "受益匪浅" }
        ]
    }
    ```
    我们可以把JSON解析为对象，于是可以通过`.`来访问其属性

#### 前端第二部分总结

- `pages/`里上次没搞懂的四个文件都大量使用了State与useEffect，与后端的交互是通过封装文件`client.js`中的函数实现的，在理解了State与useEffect的前提下就比较容易理解了
- `client.js`连接前后端的需求。

## 3. 后端


!!! note "相关学习材料"
  [FastAPI 菜鸟教程](https://www.runoob.com/fastapi/fastapi-tutorial.html)  
  [HTTP 简单介绍](https://www.runoob.com/http/http-tutorial.html)

### FastAPI 基本认识

FastAPI 是一个 Python 的现代 Web 框架，核心概念有三个：

- **路径操作（Path Operation）**：一个 URL 路径 + 一个 HTTP 方法 = 一个接口。比如 `@app.get("/posts/")` 就是"当有人 GET 请求 `/posts/` 时，执行下面这个函数"。
- **依赖注入（Dependency Injection）**：函数参数里声明 `db: Session = Depends(get_db)`，框架会自动帮你创建数据库连接，函数结束自动关闭，不用手动管理周期。
- **自动文档**：只要你用 Pydantic 定义了请求/响应的数据模型，FastAPI 自动生成 Swagger UI（`/docs`）和 ReDoc（`/redoc`），可以直接在浏览器里调试所有接口，这个功能我觉得很方便。

```python
# 一个最小 FastAPI 应用
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def root():
    return {"message": "Hello World"}
```

执行 `uvicorn app.main:app --port 8000` 启动，浏览器打开 `http://127.0.0.1:8000` 就看到 JSON 响应。

### 后端的关键概念

#### 1. 路由 = 路径 + 方法

FastAPI 用装饰器把函数变成接口，以 CRUD 为例：

```python
@app.get("/posts/")        # GET 请求 → read
@app.post("/posts/")       # POST 请求 → create
@app.put("/posts/{slug}")  # PUT 请求 → update
@app.delete("/posts/{slug}")  # DELETE 请求 → delete
```

路径参数用 `{}` 包裹，直接作为函数参数传入：

```python
@router.get("/{slug}")
def get_single_post(slug: str, db: Session = Depends(get_db)):
    # slug 直接从 URL 里取，比如 /posts/my-first-post → slug = "my-first-post"
    ...
```

#### 2. Pydantic 模型 = 数据形状

前后端传数据是通过 JSON 数据传输的，后端如何确定 JSON 的格式正确？由 Pydantic 负责提前定义"数据应该长什么样"的一个模板，FastAPI 拿到 JSON 后会自动跟模板比对：

```python
from pydantic import BaseModel
from typing import List, Optional

class PostCreate(BaseModel):
    title: str                      # 必填
    summary: Optional[str] = ""     # 可选，默认空
    content: str                    # 必填
    is_pinned: bool = False         # 可选，默认 False
    tags: List[str] = []            # 可选，默认空数组
```

简单地说，Pydantic 模型对 JSON 文件进行格式检查，如果数据有问题可以直接返回 422 错误。相当于快递站的**包裹规格检查器**——规定了包裹必须是特定形状，不符合的直接拒收。

#### 3. ORM = 用 Python 对象操作数据库

在 [simpletasker](https://github.com/hubery258/SimpleTasker) 项目中我得自己手动写 SQL 语句，这次 vibe coding 的 blog 选择启用 ORM：把数据库映射为 Python 类，把 SQL 操作改成 Python 方法调用，可以算是一种"更高层的抽象"（黑箱）：

```python
# 定义表结构（models.py）
class Post(Base):
    __tablename__ = "posts"

    id = Column(Integer, primary_key=True)
    title = Column(String(255), nullable=False)
    content = Column(Text, nullable=False)
    # ...
```

```python
# 操作数据库（crud.py）
# 查询
db.query(Post).filter(Post.slug == "hello").first()

# 创建
db_post = Post(title="Hello", content="World")
db.add(db_post)
db.commit()
```

SQLAlchemy 会自动把 `.filter(Post.slug == "hello")` 翻译成 `SELECT * FROM posts WHERE slug = 'hello'`。你操作的是一个**受控的黑箱**——不需要知道里面 SQL 长什么样，只需要用它暴露出来的 Python 方法来操作。

#### 4. 依赖注入 = 自动管理资源

```python
def get_db():
    db = SessionLocal()
    try:
        yield db       # 把 db 交给路由函数
    finally:
        db.close()     # 请求结束自动关闭

@router.get("/")
def list_posts(db: Session = Depends(get_db)):
    return db.query(Post).all()
```

每次请求到来：创建 session → 执行查询 → 请求结束 → 自动关闭。不用写任何 try/finally。

#### 5. 多对多关系

文章和标签是多对多：一篇文章可以有多个标签，一个标签下可以有多篇文章。需要一张中间表：

```python
post_tags = Table(
    "post_tags",
    Base.metadata,
    Column("post_id", ForeignKey("posts.id")),
    Column("tag_id", ForeignKey("tags.id"))
)
```

然后通过 `relationship()` 让 ORM 自动处理关联查询，你只需要 `post.tags` 就能拿到一篇文章的所有标签，不需要手写 JOIN。

---

### 具体项目分析

#### `main.py`——应用入口

做四件事：

1. **创建 FastAPI 实例**，设定 title、description
2. **配置 CORS 跨域**：前端跑在 `:5173`，后端在 `:8000`，浏览器默认禁止跨域请求。`CORSMiddleware` 告诉浏览器"我允许 `localhost:5173` 来调我"
3. **注册路由**：`app.include_router(posts.router)` 把 `routers/posts.py` 里的所有接口挂载到应用上
4. **启动时自动建表**：`Base.metadata.create_all(bind=engine)` 扫描所有继承自 `Base` 的类，自动在 SQLite 里创建对应的表

#### `database.py`——数据库连接

核心就三行：

```python
engine = create_engine("sqlite:///./blog.db")     # 创建引擎，指向 SQLite 文件
SessionLocal = sessionmaker(bind=engine)           # Session 工厂
Base = declarative_base()                          # ORM 基类
```

`connect_args={"check_same_thread": False}` 是 SQLite 特有配置——SQLite 默认不允许跨线程使用同一个连接，FastAPI 的多线程模式下需要关掉这个限制。

#### `models.py`——表结构

定义了 3 张表：

| 表名 | 类名 | 关键字段 |
|---|---|---|
| `posts` | `Post` | id, title, slug(唯一索引), summary, content(Markdown), is_pinned, created_at, updated_at |
| `tags` | `Tag` | id, name(唯一索引) |
| `site_meta` | `SiteMeta` | id, about_title, about_content |

外加一张多对多中间表 `post_tags`。

这里有一个值得注意的设计：slug 是文章的 URL 友好标识符。比如标题"我的第一篇文章" → `slugify()` → `"wo-de-di-yi-pian-wen-zhang"`，用作 URL `/posts/wo-de-di-yi-pian-wen-zhang`。`crud.py` 里的 `generate_unique_slug()` 会自动处理重名，在后面加 `-1`、`-2`。

#### `schemas.py`——数据的"形状"

前端和后端之间的**合同**，也就是我们定义格式之处：

- **请求体**（前端→后端）：`PostCreate`、`PostUpdate`、`SiteMetaUpdate`
- **响应体**（后端→前端）：`PostRead`（详情页返回全文）、`PostListItem`（列表页不返回正文，更轻量）、`TagRead`、`SiteMetaRead`

#### `crud.py`——数据库操作

路由只负责"接收请求 → 调 crud → 返回响应"，所有数据库操作都在这里：

```python
def create_post(db, post_data):
    slug = generate_unique_slug(db, post_data.title)    # 1. 生成唯一 slug
    tag_objects = get_or_create_tags(db, post_data.tags) # 2. 标签不存在就自动创建
    db_post = Post(title=..., slug=..., tags=tag_objects) # 3. 组装 Post 对象
    db.add(db_post)
    db.commit()    # 4. 写入数据库
    return db_post
```

`get_or_create_tags` 先查是否存在，存在就复用，不存在就新建——这样不会出现重复标签。

文章列表排序逻辑：

```python
posts = sorted(
    all_posts,
    key=lambda p: (not p.is_pinned, -p.created_at.timestamp())
)
# 置顶的在前（not True = 0 < not False = 1），
# 同级别按创建时间倒序（负数让最新的在前）
```

#### `routers/`——API 接口层

每个 router 文件就是一个 `APIRouter` 实例，有自己的 `prefix` 和 `tags`：

```python
router = APIRouter(prefix="/posts", tags=["posts"])
```

这样在文件里写 `@router.get("/")` 实际映射到 `/posts/`，写 `@router.get("/{slug}")` 映射到 `/posts/{slug}`。

**管理员保护的实现：**

```python
@router.post("/")
def create_new_post(
    post: PostCreate,
    db: Session = Depends(get_db),
    x_admin_secret: str | None = Header(default=None)  # 从请求头取
):
    if x_admin_secret != os.getenv("ADMIN_SECRET"):
        raise HTTPException(status_code=401, detail="管理员密钥错误")
    return crud.create_post(db, post)
```

前端在请求头里带 `X-Admin-Secret`，后端和 `.env` 里配的 `ADMIN_SECRET` 比对。简单但有效——适合不需要复杂登录系统的场景。

---

### 交互全程

以"用户访问首页"为例，追踪数据流：

```
浏览器输入 https://blog.ramenboy.cc/
        │
        ▼
React Router 匹配 "/" → 渲染 HomePage 组件
        │
        ▼
HomePage 的 useEffect 调用 getPosts() 和 getTags()
        │
        ▼
client.js 发出 fetch("http://127.0.0.1:8000/posts/")
        │
        ▼
FastAPI 收到 GET /posts/
        │
        ▼
路由匹配 → routers/posts.py → list_posts()
        │
        ▼
Depends(get_db) 创建数据库 session
        │
        ▼
crud.get_posts(db)
  → db.query(Post).all()
  → 排序（置顶优先，时间倒序）
        │
        ▼
FastAPI 把返回的 Post 对象按 PostListItem schema 过滤
（不返回 content 正文，只返回标题/摘要/日期/标签）
        │
        ▼
JSON 响应返回给前端
        │
        ▼
HomePage 的 setPosts(data) 更新 state
        │
        ▼
React 重新渲染，文章列表显示在页面上
```

### 阶段性总结

1. **后端本质上就是"接收 HTTP 请求 → 查/改数据库 → 返回 JSON"**。框架只是做一点黑箱包装操作，省掉了 HTTP 解析、JSON 序列化、连接管理这些脏活累活。
2. **前后端协作的核心是契约**：Pydantic schema 就是契约。前端按契约发数据，后端按契约返回数据，两边独立开发互不阻塞。写到这又想起前段时间学的 DDD，有互通性。
3. **三层分离**：`models`（表结构）、`schemas`（接口形状）、`crud`（数据库操作）各司其职，路由只做调度不写 SQL。
4. **依赖注入是精髓**：数据库 session、认证信息、配置项都可以用 `Depends()` 注入，代码简洁且易于测试。
5. **SQLite 适合小项目**：零配置、单文件、不需要单独安装数据库服务。个人博客完全够用。以后做高访问量需求的项目时可以无痛迁移到 PostgreSQL（只改 `database.py` 里一行连接字符串）。

## 4. 部署

### 从 localhost 到公网：一张图看懂

```
电脑上的开发环境                    生产环境（云服务器）
┌─────────────────┐                ┌──────────────────────────┐
│ React :5173     │                │  用户浏览器              │
│ FastAPI :8000   │   git push     │      │                   │
│ SQLite blog.db  │ ──────────────→│  https://blog.ramenboy.cc│
│                 │  GitHub Actions│      │                   │
└─────────────────┘                │  Nginx (反向代理)        │
                                   │   ├── /api/* → :8000     │
                                   │   └── /*     → 静态文件  │
                                   │  FastAPI :8000           │
                                   │  SQLite blog.db          │
                                   └──────────────────────────┘
```

下面一个一个拆解这中间的每一步。

### 1. 域名：互联网的门牌号

服务器在互联网上有一个 IP 地址，比如 `43.139.xxx.xxx`。但没人记得住一串数字，所以有了**域名**。

DNS（Domain Name System）就是互联网的电话簿：

```
blog.ramenboy.cc
        │
        ▼
DNS 服务器查到：这个域名指向 43.139.xxx.xxx
        │
        ▼
浏览器向 43.139.xxx.xxx 发起请求
```

#### 实际需要做什么(下面直接以我为例)

1. 在腾讯云买了一年的 `ramenboy.cc`
2. 在 DNS 管理后台加了一条 **A 记录**：

| 主机记录 | 记录类型 | 记录值 |
|---|---|---|
| `blog` | A | `43.139.xxx.xxx`（我的服务器 IP） |

3. 等几分钟 DNS 生效后，`blog.ramenboy.cc` 就能解析到我的服务器了

> 使用`@` 主机记录代表根域名本身（`ramenboy.cc`），`blog` 代表二级域名（`blog.ramenboy.cc`）。

### 2. 服务器：代码住在哪里

#### 云服务器就是一台远程 Linux 电脑

在腾讯云买了一台轻量应用服务器,Ubuntu 系统。本质上它就是一台永远不关机的远程电脑。

#### SSH：远程操控服务器

```bash
ssh ubuntu@43.139.xxx.xxx
```

输入密码后，我们就登录到了远程 Linux 终端。之后的所有操作——装软件、传代码、启动服务——都在这上面完成,我在服务器上直接clone之后进入前端 `npm run build` 之后生成 `dist/` 文件夹，里面是纯 HTML+JS+CSS，Nginx 直接托管即可，**不需要在服务器上跑 `npm run dev`**。

### 3. 反向代理：Nginx 把请求转给应用

#### Nginx

FastAPI 跑在服务器的 `8000` 端口上，但用户不想（也不应该）在浏览器里输入 `blog.ramenboy.cc:8000`.Nginx 就是一个**门童**，站在 80（HTTP）和 443（HTTPS）端口，根据请求路径转发给不同的后端：

```
用户请求 https://blog.ramenboy.cc/posts/
        │
        ▼
Nginx (监听 443 端口)
        │  判断：路径 /posts/ 不以 /api 开头
        ▼
返回前端静态文件（dist/index.html）

用户请求 https://blog.ramenboy.cc/api/posts/
        │
        ▼
Nginx (监听 443 端口)
        │  判断：路径以 /api 开头
        ▼
转发给 localhost:8000 (FastAPI)
```

对应的 Nginx 配置大致是：

```nginx
server {
    listen 443 ssl; # 监听
    server_name blog.ramenboy.cc;

    # 前端静态文件
    location / {
        root /home/ubuntu/dreamland/frontend/dist;
        try_files $uri /index.html;   # SPA 路由回退
    }

    # 后端 API
    location /api/ {
        proxy_pass http://127.0.0.1:8000/;  # 转发给 FastAPI
    }
}
```

#### 关键概念

| 概念 | 解释 |
|---|---|
| 反向代理 | Nginx 替用户去问后端，再把结果返回给用户。用户不知道后端的存在 |
| 端口 | 一台服务器有 65535 个端口，80/443 是 HTTP/HTTPS 的默认端口（浏览器会自动补） |
| `try_files` | 先找文件，找不到就回退到 `index.html`，React SPA 路由必须这样配 |

### 4. HTTPS

HTTP 是明文传输，中间人可以偷看/篡改内容。HTTPS = HTTP + SSL/TLS 加密。

#### Let's Encrypt 免费证书

安装 Certbot，一行命令搞定：

```bash
sudo certbot --nginx -d blog.ramenboy.cc
```

Certbot 会自动：
1. 向 Let's Encrypt 申请证书
2. 修改 Nginx 配置加上 SSL
3. 设置定时任务，每 90 天自动续期

### 5. 用 systemd 把应用变成系统服务

执行到目前我们想要运行应用还是要手动启动,断开remote链接后服务还会停。接下来使用systemd, 创建一个服务文件 `/etc/systemd/system/dreamland.service`：

```ini
[Unit]
Description=Dreamland Blog Backend
After=network.target

[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/dreamland/backend
ExecStart=/home/ubuntu/dreamland/backend/venv/bin/uvicorn app.main:app --host 0.0.0.0 --port 8000
Restart=always

[Install]
WantedBy=multi-user.target
```

然后：

```bash
sudo systemctl daemon-reload    # 重新加载配置
sudo systemctl enable dreamland # 开机自启
sudo systemctl start dreamland  # 立即启动
sudo systemctl status dreamland # 查看状态
```

这样后端就变成了一个**系统级别的后台服务**——崩溃自动重启、服务器开机自动启动，不用守着remote终端。

### 6. CI/CD：自动部署

#### 传统流程 vs CI/CD

```
传统：本地改代码 → 手动 scp 上传 → SSH 登录 → 手动重启服务
CI/CD：本地改代码 → git push → 自动部署 → 完成
```

#### 我的 GitHub Actions 配置

`.github/workflows/deploy.yml`：

```yaml
name: Deploy Dreamland
on:
  push:
    branches:
      - main            # 推送到 main 分支时触发

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup SSH
        run: |
          echo "${{ secrets.SSH_PRIVATE_KEY }}" > ~/.ssh/id_ed25519
          chmod 600 ~/.ssh/id_ed25519
          ssh-keyscan -H ${{ secrets.SERVER_HOST }} >> ~/.ssh/known_hosts

      - name: Deploy to server
        run: |
          ssh ${{ secrets.SERVER_USER }}@${{ secrets.SERVER_HOST }} \
            "cd /home/ubuntu/dreamland && ./deploy.sh"
```

#### 服务器上的 `deploy.sh`

```bash
#!/bin/bash
cd /home/ubuntu/dreamland
git pull origin main                    # 拉取最新代码
cd frontend && npm install && npm run build  # 构建前端
sudo systemctl restart dreamland        # 重启后端
```

### 7. 全链路回顾

```
用户浏览器
    │  https://blog.ramenboy.cc
    ▼
DNS 解析 → 你的服务器 IP (43.139.xxx.xxx)
    │
    ▼
Nginx (端口 443)
    │  SSL 证书验证 → HTTPS 加密连接建立
    │  路径判断：
    │  ├── /posts/* /tags/* / → 返回前端静态文件
    │  └──                  → FastAPI (127.0.0.1:8000)
    │
    ▼
FastAPI 处理请求 → SQLAlchemy → SQLite blog.db
    │
    ▼
JSON 响应 → Nginx → 用户浏览器 → React 渲染页面
```

### 阶段性总结

1. **域名 + DNS** = 给 IP 取个名字，让用户不用记数字
2. **Nginx 反向代理** = 所有流量统一从 443 进来，再按路径分发给不同后端
3. **HTTPS** = 用 Let's Encrypt 免费证书，一行命令搞定
4. **systemd** = 把 FastAPI 变成系统服务，崩溃自动重启
5. **GitHub Actions** = `git push` 后自动 SSH 到服务器执行部署脚本
6. **备案** = 国内服务器必须备案，境外不需要

