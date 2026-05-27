# Week9 Flask
*前两讲直接抄课程note，自己整理的部分其实比较少，这一讲尝试少复制粘贴，多考虑一下怎么用自己的话讲明白且简洁*

相关链接：[Flask documentation](https://flask.palletsprojects.com/)
[AJAX documentation(about API)](https://api.jquery.com/category/ajax/)
[JSON documentation](https://www.json.org/json-en.html)

***
## Flask
- `http-server`主要实现静态网页，是用户端从服务器段下载下来一个完整网页，flas（一个third-party library）可以用来搭建一个同一页面互动app，by Python

- Run Flask by executing `flask run` 
    - 需要基本文件：`app.py`,`requirements.txt`
    - 预装一些库：`pip install -r requirements.txt`,有一个必须要的是`Flask`。
    - `app.py`实现web application的核心操作，基本格式可以看*src9*里的一系列*hello*文件

- 强大的**templates**：使app return一个模板，可以含html，css，javascript，见*hello2*开始，主要要自己建一个文件夹`templates`
    - 有时我们要从`app.py`中向其他模板传递一些信息，在模板中就要用`{{ name }}`这样的表示一个placeholder

- *hello 3~5*：`request.args.get`接受从网页中`get`状态获取的值，而如果用了`request.args.get("name", "world")`,在未收到值的情况下就会发*world*。
    - 当然，可以直接adding `?name=[Your Name]` to the base URL in your browser’s URL bar.让他强行读取，注意：
    1. 要在网址后自己补一个`/`后再输入，大部分时候浏览器会默认隐藏
    2. **URL 里的 `?name=xxx` 中的 `name` 是一个 key，`xxx` 是它的 value，`request.args.get("name", "world")` 中的 `"name"` 也是 key，用来取这个 value。取出来的 value 会赋给 Python 变量 `name`。**

***

## Forms
*tips:form=表单，是一个让用户输入数据的区域*<br>
正常人不会在网址上输入各种信息，我们学习如何实现form之间信息的传递。

- *hello6*的知识：
    - `index.html`里的`action`是指定路由传输方向，`meta`这一行实现不同设备的显示兼容，查看可以用开发者工具

***

# Templates

- 先来一个`layout.html`作为新模板，*hello7*里有代码示例,总之可以减少重复工作。
    
- **`<form>` 明确告诉浏览器：提交后就去 /greet**。
```html
<form action="/greet" method="get">
```
这里的 `action="/greet"` 就是在说：

> “当用户点击 submit 按钮时，把表单里的数据用 GET 方法提交到 /greet 这个 URL。”

所以流程是这样的：
### 1) 用户点击 submit  
浏览器会把 `<input>` 里的内容打包成查询参数：
```
name=你输入的内容
```
### 2) 浏览器根据 `<form action="/greet">`  自动访问：
```
/greet?name=你输入的内容
```
### 3) Flask 的 `/greet` 路由接收到请求,执行
```python
return render_template("greet.html", name=request.args.get("name", "world"))
```

***

## Request Methods
- `POST`比`GET`安全，它不会把交的信息放到网址上:
```python
@app.route("/greet", methods=["POST"])
def greet():
    return render_template("greet.html", name=request.form.get("name", "world"))
```
注意要在methods里的写法，但是最好其实写成`["POST","GET"]`来保证两种输入方式，在用post时就要写`request.form.get`(*hello8*)
- **合并写法**见hello9

- 注意，需要考虑啥都不输入的情况，见*hello10*：
```
    {% if name %}
        {{ name }}
    {% else %}
        world
    {% endif %}
```
很诡异，但是也好掌握，最后注意endfor或者endif即可

***

## 项目实战1：Frosh IMs（SQL）

- 除开templates之类的文件，要储存图片就新建一个`static`文件夹
- flask/python里的写法：`if not request.form.get("name") or not request.form.get("sport"):`如果不存在...或者没有...
- 想要实现选择，可以使用`select`,`option`这两个tag，见froshims1，类似地，可以用`input`中的`type="radio"`实现圆点式选择(*froshims2*)

- 为了真正获取数据，第一种可以选择在app内下全局变量（*froshims3*）
- *以上方法都只用到了html，前端，很容易被人修改html黑入，并且数据储存也不持久，关闭服务器就没了，于是：*

### **SQL in Flask**
- SQL中的占位符：`?`
- 一些操作见`froshims4`.

### Cookies and Session
- 一点知识：`app.py` is considered a *controller*. A *view* is considered what the users see. A *model* is how data is stored and manipulated. Together, this is referred to as ***MVC*** (model, view, controller).
- Cookies是一些装着很大随机字符数字的文件，存在自己电脑里每次打开网站调用，提醒网站：我已经验证过了。用这个cookie授权就是session：
```
GET / HTTP/2
Host: accounts.google.com
Cookie: session=value
```
`session` 是一个*key*或理解为一个全局变量，然后在这之中对应名字对应自己的值，是一个字典，这个功能需要加入一个新的库，`Flask-Session`,见`login`

- 注意`login`的`index.html`里*if-else*的神经写法
    - 这个程序还有很多可以学习的地方，我看代码扫了一遍，感觉差不多。   

- seesion 的更多：见`store`

***

## 实战2： Shows
- Shows0,1都还算好理解，但是都是输入之后再去跳转网页匹配

- `shows2`引入JavaScript，可以实现网页内动态查询，关键代码：
```html
<script>
    let input = document.querySelector('input');
    input.addEventListener('input', async function() {
        let response = await fetch('/search?q=' + input.value);
        let shows = await response.text();
        document.querySelector('ul').innerHTML = shows;
    });
</script>
```
- `await`和`async`,`fetch`等等函数的意义我不求甚解，目前知道能实现即可。有空时可以研究。

### API
- 定义小课堂：An *application program interface* or *API* is a series of specifications that allow you to interface with another service. For example, we could utilize IMDB’s API to interface with their database.

### JSON
- 定义小课堂：*JavaScript Object Notation* or *JSON* is a text file of dictionaries with keys and values. This is a raw, computer-friendly way to get lots of data.
- 不是cs50真的越上越快，我怀疑这一坨本来就是打算让你自学的，只是给你展示一下。