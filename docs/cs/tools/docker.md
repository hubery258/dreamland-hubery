# docker

!!! info "相关资料"
	安装好后运行指令`docker run -d -p 80:80 docker/getting-started`



## **“集装箱思维模型”**

> 来自ai加上一点我自己的理解,我觉得还是有点质量，如果你之前就接触过docker，且ai帮你写过dockerfile帮你运行过docker指令或许从这里入手更方便

### 1. 核心三件套

Docker 世界只有三个主角，请把下面这个**“厨房做菜”**类比刻在脑子里：

- **Dockerfile（菜谱）**：告诉别人怎么做这道菜（需要什么食材、先放油还是先放盐）。**这是配置文件，是文本。**
- **Image（食材包/午餐肉罐头）**：按照菜谱做好的**只读成品**。它被存在你的硬盘上，不吃（运行）就不会变。**（类比：你下载的 `.iso` 镜像文件）**
- **Container（正在吃的这碗饭）**：把午餐肉罐头打开，在锅里加热产生的**动态进程**。它是活的，会占用内存和CPU，且**它里面的任何修改（比如撒了辣椒粉），在罐头（Image）里并不存在**。

**初学者最大的误区就是混淆 Image 和 Container。** 
记住：**Image 是类（Class），Container 是实例（Object）。** 你可以用同一个 Image 跑出成千上万个互不干扰的 Container。

---

### 2. 你的数据去哪了？

你之前用 AI 写的 `docker run` 命令里，肯定见过 `-v` 挂载。为什么需要它？因为**Container 是一个“一次性”的沙盒**。如果不用 `-v`，你在容器里创建的文件，随着你输入 `docker rm` 删除容器，**数据就彻底灰飞烟灭了**。

**如果没有`-v`指令，那么容器内的数据保存在容器内，rm就直接消失了，但如果有些数据有保存需求，会提前在dockerfile里写出需要挂载到某个位置的占位符(不固定位置)(VOLUME)，然后由运行者在运行image时自行指定这个container的需保存数据的挂载位置，然后文件就保存到宿主机中了。** 当然，如果数据想持久化，由运行者在 docker run 时，强行用 -v 把宿主机的文件夹塞进去，容器里的对应位置就被接管了(`docker run -v /my/host:/container`)。Dockerfile 写不写 VOLUME，并不影响这个“接管”动作的发生，更多的是dockerfile作者对运行者的提醒与建议“最好外部储存哦”

??? example "小测试"
	假如我写了一个 Dockerfile，里面有一句 `VOLUME ["/app/logs"]`，然后我执行：`docker run -v /Users/me/logs:/app/logs my-image`,请问：容器里的 `/app/logs` 和宿主机里的 `/Users/me/logs`，到底是谁覆盖谁？如果容器启动时里面本来就有一个 `test.txt`，我挂载后还能看到它吗？
	容器内的 `/app/logs` 被宿主机的 `/Users/me/logs` “覆盖/屏蔽”掉了。  
	也就是说，宿主机是“强势方”，容器是“被动方”。挂载的一瞬间，容器里的那个目录就直接指向了宿主机。你看不到容器里原本自带的 `test.txt`，是因为它被隐藏了.

---

### 3. 你必须死磕的 5 个基础指令（其他都是衍生）

不用背几百个，入门先掌握这 5 个，足以覆盖 80% 场景：

| 指令 | 作用 | 你的直观理解 |
| :--- | :--- | :--- |
| **`docker build`** | 把 `Dockerfile` 变成 `Image` | 按照菜谱，把食材打包成罐头（存到硬盘） |
| **`docker run`** | 从 `Image` 创建并启动一个 `Container` | 打开罐头，开火炒菜（让进程跑起来） |
| **`docker ps`** | 查看当前正在运行的容器列表 | 看看厨房里哪口锅正在冒烟（加 `-a` 看所有历史锅） |
| **`docker stop`** | 停止运行中的容器 | 关火，但锅（容器文件）还在硬盘上 |
| **`docker rm`** | 删除停止的容器 | 把锅扔了，彻底清理（释放少量硬盘空间） |

---

### 4. Dockerfile 

其实它的灵魂就两条：

1. **FROM（起点）**：我要基于哪个现成的罐头（比如 `python:3.9`）来做。
2. **RUN / COPY / CMD（动作）**：在罐头里新建文件夹、复制代码、执行命令。

**写 Dockerfile 的唯一心法（层缓存机制）：**
Dockerfile 里每一行都会生成一个**中间层**。为了加快下次构建速度，**越不容易变动的指令放在越上面**。
**正确顺序：** 先把 `requirements.txt` 复制进去，执行 `RUN pip install`（这层很耗时会缓存），最后才 `COPY . /app`（代码天天变，放最后）。

> **记住这个感觉：Docker 容器就是一个极其轻量的、独立的 Linux 操作系统进程。** 你进入它内部后，就当它是一台刚装好的新电脑。


## 入门

`docker run -d -p 80:80 docker/getting-started`

- `-d` 在后台以分离模式运行容器
- `-p 80:80` 将主机的80端口映射到容器内的80端口
	- 在 Docker 绝大部分指令里，冒号 `:` 左边永远代表“宿主机（外部）”，右边永远代表“容器内部（内部）”。你可以把它理解为 “把外面的东西，接到里面去”。
- `docker/getting-started-` 使用的image

容器(container) :  a container is another process on your machine that has been isolated from all other processes on the host machine

### dockerfile example

以getting-started里的文件为例写的dockerfile:
```
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
```

| 指令 | 功能 | 为什么这么写？ |
| :--- | :--- | :--- |
| **`FROM node:18-alpine`** | 去仓库把 Node.js 18 的轻量级基础包（Alpine 版）下载下来，当做我的“空白锅底”。 | 必须放在第一行，决定了你的菜系（环境）。 |
| **`WORKDIR /app`** | **（官方文档完全没解释这句，但极其重要！）** 在容器里创建一个叫 `/app` 的文件夹，并**把这里设为默认的“当前工作目录”**。 | 就像打开终端后先敲 `cd /app`。如果不写，后面 `COPY . .` 会把文件复制到根目录（`/`）去，乱成一锅粥。 |
| **`COPY . .`** | 把**你电脑当前文件夹**（第一个 `.`）里的所有东西，复制到**容器里的 `/app`**（第二个 `.`）下。 | 第一个 `.` 是“外面”，第二个 `.` 是“里面（当前工作目录）”。 |
| **`RUN yarn install --production`** | 在容器内执行 `yarn install`，只安装生产环境需要的依赖包。 | 这步是在**做镜像（罐头）时**执行的，装好的依赖会被打包进罐头里。 |
| **`CMD ["node", "src/index.js"]`** | 当这个罐头被打开（容器启动）时，自动执行 `node src/index.js` 来跑起服务。 | 注意！这步**不是**在做镜像时执行，而是**未来每次 `docker run` 启动时**才执行。 |

### build and run

```
docker build -t getting-started .
```

- **`docker build`**：核心命令，意思是“照着 Dockerfile 给我造一个镜像（罐头）”。
- **`-t getting-started`**：`-t` 就是 **Tag（标签）**，相当于给造好的罐头**贴个名字**叫 `getting-started`。以后敲 `docker run getting-started` 就直接用它了。
- **命令末尾的 `.`（这是初学者漏掉率 99% 的盲区！）**：
  - 官方说“Docker should look for the Dockerfile in the current directory”，翻译是“Docker 会在当前目录找 Dockerfile”。
  - **但更深层的本质是**：这个 `.` 代表 **“构建上下文（Build Context）”**。意思是你把**当前整个文件夹**打包“上传”给 Docker 引擎，让它在这个环境里去执行 `COPY` 指令。如果没有这个 `.`，Docker 连 `COPY . .` 要拷哪些文件都不知道.如果在执行过程中Docker 发现本地没有 `node:18-alpine`，会先下载（看到一堆layer），然后逐行执行你的指令。

```
docker run -dp 3000:3000 getting-started
```

### update

更新代码后我们要上新的容器，先停止旧容器，然后删除：

1. 用`docker ps`查询容器id
2. 停止:`docker stop <the-container-id>`
3. `docker rm <the-container-id>`删掉，也可以两步一起:`docker rm -f <the-container-id>`

### database

可以通过**卷(Volumes)**将容器中特定文件系统路径连接到主机。如果容器中的某个目录被挂载，该目录的变化也会在主机上被看到。如果我们在容器重启时挂载同一个目录，我们会看到相同的文件。卷主要有两种类型,从命名卷开始：

#### named volume

1. `docker volume create todo-db`,注意在这个命令出现之前在运行状态的容器是没有使用持久化数据的
2. 重新启动并挂载：`docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started`
3. 此后如果删除容器再换新的，数据也还在。

!!! note "named volume实际储存位置"
	使用`docker volume inspect xx`查询,belike:
	```
	docker volume inspect todo-db
	[
    	{
        	"CreatedAt": "2026-07-27T03:54:59Z",
        	"Driver": "local",
        	"Labels": null,
        	"Mountpoint": "/var/lib/docker/volumes/todo-db/_data",
        	"Name": "todo-db",
        	"Options": null,
        	"Scope": "local"
    	}
	]
	```


#### bind mounts

有时为了“热更新”（Dev 模式），我们要自己指定数据库定位，且如果我们直接把项目代码文件夹挂进容器，那么在电脑上改一行代码，容器里的代码立刻同步改变，配合 nodemon 工具，容器里的服务自动重启，你刷新浏览器就能看到变化——再也不用改一次代码就`docker build` 一次了

!!! inline end warning
	如果容器有文件，空文件夹挂载：named volumes容器里的内容会复制进卷里（初始化填充），而如果采用bind mounts: 容器里的内容被隐藏.

1. `docker run -dp 3010:3000 -w /app -v "${PWD}:/app" node:18-alpine sh -c "yarn install && yarn run dev"` 

| 参数 | 翻译成人话 |
| :--- | :--- |
| `docker run` | 启动一个新容器 |
| `-dp 3010:3000` | **`-d`** 后台运行（别卡我终端）；**`-p`** 电脑的 3010 接容器里的 3000 |
| **`-w /app`** |  设定容器内的**当前工作目录**为 `/app`。相当于一进容器就自动执行 `cd /app`，否则后面的命令不知道在哪跑。 |
| **`-v "$(pwd):/app"`** | **核心！** 把**你电脑当前文件夹**（`$(pwd)` 自动获取绝对路径,注意`()`是linux和macos写法，我提供的是win写法）挂载到容器的 **`/app`**。这就是“绑定挂载”，实现了代码同步。 |
| `node:18-alpine` | 使用 Node.js 18 的轻量级镜像作为运行环境。 |
| `sh -c "yarn install && yarn run dev"` | 在容器里启动一个 `sh` 终端，执行两件事：**先**装依赖（`yarn install`），**然后**启动开发模式（`yarn run dev`，即 `nodemon` 热更新工具）。 |

2. `docker logs -f <container-id`可以查询log,`-f`表示follow，持续查看实时日志

### networking 与 Docker Compose

> If two containers are on the same network, they can talk to each other. If they aren't, they can't.

容器默认是孤立运行的，对同一台机器上的其他进程或容器一无所知，除非我们建立网络，在同一个网络里，容器名就是“域名”.

1. 新建一个`docker-compose.yml`
2. 代码:
```
services:
	# node.js服务
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

	# mysql 数据库服务
  mysql:
    image: mysql:8.0
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment: 
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

# 这一块是“注册”命名卷，告诉 Docker 这个卷我要留着存数据
volumes:
  todo-mysql-data:
```
3. 启用这个网络下的所有服务: `docker compose up -d`
4. `docker compose down` 数据卷（数据库文件）默认不会被删，留着下次用,如果要清空加入`--volumes` 参数

[ai设计的实战](http://simp.ly/p/m6HlCy)
