# luoxi-wp
一个基于 FastAPI 构建的轻量级文件管理服务，同时支持 Web UI 和 WebDAV 协议

## 功能特性

- **Web UI**: 提供直观的文件管理界面，支持文件上传、下载、创建目录、重命名和删除
- **WebDAV 支持**: 支持标准 WebDAV 协议，可以通过系统文件管理器或 WebDAV 客户端访问
- **用户管理**: 支持多用户，区分管理员和普通用户权限
- **安全认证**: 使用 PBKDF2 密码哈希算法，支持会话管理
- **Docker 部署**: 提供 Dockerfile，方便快速部署

## 技术栈

- Python 3.11+
- FastAPI
- Uvicorn
- Pydantic

## 快速开始

### 使用 Docker（推荐）

```bash
# 构建镜像
docker build -t fast-web-file .

# 运行容器
docker run -d \
  -p 8080:8080 \
  -p 1900:1900 \
  -v /path/to/data:/app/data \
  fast-web-file
```

### 本地运行

```bash
# 安装依赖
pip install -r requirements.txt

# 运行服务（同时启动 Web 和 WebDAV）
python main.py

# 仅启动 Web 服务
python main.py --mode web

# 仅启动 WebDAV 服务
python main.py --mode dav
```

## 访问服务

### Web UI
- 地址: `http://localhost:8080`
- 默认管理员账号: `admin` / `admin123`
- 默认演示账号: `demo` / `demo123`

### WebDAV
- 地址: `http://localhost:1900`
- 使用任意用户账号登录

## 配置选项

### 环境变量

| 变量名 | 默认值 | 说明 |
|--------|--------|------|
| `DATA_ROOT` | `/app/data` | 用户数据存储目录 |
| `USERS_FILE` | `/app/users.json` | 用户配置文件路径 |
| `WEB_HOST` | `0.0.0.0` | Web 服务绑定地址 |
| `WEB_PORT` | `8080` | Web 服务端口 |
| `DAV_HOST` | `0.0.0.0` | WebDAV 服务绑定地址 |
| `DAV_PORT` | `1900` | WebDAV 服务端口 |
| `SERVICE_MODE` | `all` | 运行模式：`all` / `web` / `dav` |

### 命令行参数

```bash
python main.py --help
```

## API 接口

### 用户认证
- `POST /api/login` - 用户登录
- `POST /api/logout` - 用户登出
- `GET /api/me` - 获取当前用户信息

### 文件操作
- `GET /api/files` - 列出目录内容
- `POST /api/upload` - 上传文件
- `GET /api/download` - 下载文件
- `POST /api/mkdir` - 创建目录
- `POST /api/rename` - 重命名文件/目录
- `DELETE /api/delete` - 删除文件/目录

### 管理员接口
- `GET /api/admin/users` - 获取用户列表
- `POST /api/admin/users` - 创建新用户
- `PUT /api/admin/users/{username}` - 更新用户信息
- `DELETE /api/admin/users/{username}` - 删除用户

## 项目结构

```
├── main.py              # 主应用入口
├── requirements.txt     # 依赖列表
├── Dockerfile           # Docker 构建配置
├── static/              # 静态资源目录
│   ├── index.html       # 主页面
│   └── setting.html     # 设置页面
├── data/                # 用户数据目录（运行时创建）
└── users.json           # 用户配置文件（运行时创建）
```

## 安全说明

1. 密码使用 PBKDF2-SHA256 算法加密存储
2. 会话令牌使用随机生成的安全字符串
3. 路径访问进行严格的越界检查
4. 建议在生产环境中使用 HTTPS

## 许可证

Apache License 2.0
