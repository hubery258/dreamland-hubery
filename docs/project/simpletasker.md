# Simple Tasker
[项目网址](https://github.com/hubery258/SimpleTasker)

轻量的四象限任务管理应用,支持基本的任务新增（ddl，名字，描述，预计用时），修改，删除与完成，页面预览见最下方。

*more coming...*

## 快速开始

1. 克隆仓库并进入项目根目录：

   ```bash
   git clone https://github.com/hubery258/SimpleTasker.git
   cd SimpleTasker
   ```

2. 建议使用虚拟环境并安装依赖：

   - 在 macOS / Linux：

     ```bash
     python3 -m venv .venv
     source .venv/bin/activate
     pip install -r requirements.txt
     ```

   - 在 Windows (PowerShell)：

     ```powershell
     python -m venv .venv
     .\.venv\Scripts\Activate.ps1
     pip install -r requirements.txt
     ```

3. 初始化数据库（只需一次）

    有两种方式：

    - 手动：在项目根目录创建 SQLite 数据库 `task.db` 并执行下面的 SQL：

       ```sql
       CREATE TABLE tasks (
          id INTEGER PRIMARY KEY AUTOINCREMENT,
          name TEXT NOT NULL,
          ddl TEXT,
          content TEXT,
          duration INTEGER NOT NULL,
          quadrant INTEGER NOT NULL,
          completed INTEGER NOT NULL DEFAULT 0
       );
       ```

    - 使用脚本（推荐）：运行仓库内的脚本，会在项目根创建或确保 `task.db` 含有 `tasks` 表：

       ```bash
       python scripts/init_db.py
       ```

4. 启动应用（推荐使用 `flask run`）：

   ```bash
   # 在激活虚拟环境后
   export FLASK_APP=app.py        # macOS / Linux
   set FLASK_APP=app.py           # PowerShell
   flask run
   ```

   访问 http://127.0.0.1:5000/ 查看界面。

## 项目结构

```
SimpleTasker/
├── app.py                 # 应用入口，Flask 路由与后端逻辑
├── task.db                # 本地 SQLite 数据库（脚本/手动创建）
├── scripts/               # 脚本
│   └── init_db.py         # 初始化 SQLite `tasks` 表的脚本
├── templates/             # Jinja2 视图层
│   ├── layout.html        # 全局布局（header/footer）
│   ├── tasks.html         # 主页面：四象限视图与任务渲染
│   └── task_modal.html    # 新增/编辑复用模态
├── static/                # 前端资源
│   ├── tasks.js           # UI 行为与 AJAX（add/edit/delete/complete）
│   ├── style.css          # 自定义样式（四象限与卡片）
│   └── assets/            # 图片 / 字体 / 其他静态资源（可选）
├── requirements.txt       # Python 依赖
└── .gitignore
```

## 未来更新
- see on `ideas.md`