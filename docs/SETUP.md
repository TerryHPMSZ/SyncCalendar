# 开发环境设置指南

## 前置要求

- Python 3.8 或更高版本
- pip (Python 包管理器)
- Git

## 快速开始

### 1. 克隆仓库

```bash
git clone https://github.com/TerryHPMSZ/SyncCalendar.git
cd SyncCalendar
```

### 2. 创建虚拟环境

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Linux / macOS
python3 -m venv venv
source venv/bin/activate
```

### 3. 安装依赖

```bash
cd backend
pip install -r requirements.txt
cd ..
```

### 4. 配置环境变量

```bash
# 复制示例配置文件
cp backend/.env.example backend/.env

# 编辑 .env 文件并根据需要修改配置
```

### 5. 初始化数据库

```bash
cd backend

# 使用 Python 交互式 shell
python
>>> from app import create_app, db
>>> app = create_app()
>>> app.app_context().push()
>>> db.create_all()
>>> exit()

# 或使用一行命令
python -c "from app import create_app, db; app = create_app(); app.app_context().push(); db.create_all()"
```

### 6. 运行应用

```bash
# 确保在虚拟环境中
python run.py
```

应用将在 `http://localhost:5000` 运行

## 测试 API

### 使用 curl

```bash
# 1. 注册新用户
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "username": "testuser",
    "email": "test@example.com",
    "password": "password123",
    "full_name": "Test User"
  }'

# 2. 登录
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "testuser",
    "password": "password123"
  }'

# 3. 获取当前用户信息（使用上面返回的 token）
curl -X GET http://localhost:5000/api/auth/me \
  -H "Authorization: Bearer <your_token_here>"
```

### 使用 Postman

1. 导入 API 集合（待建立）
2. 设置环境变量：`base_url` = `http://localhost:5000/api`
3. 运行请求

### 使用 Python requests

```python
import requests

BASE_URL = "http://localhost:5000/api"

# 注册
response = requests.post(f"{BASE_URL}/auth/register", json={
    "username": "testuser",
    "email": "test@example.com",
    "password": "password123"
})
print(response.json())
```

## 常见问题

### 问题 1: "ModuleNotFoundError: No module named 'flask'"

**解决方案**: 确保虚拟环境已激活，并运行 `pip install -r requirements.txt`

### 问题 2: 数据库连接错误

**解决方案**: 检查 `.env` 文件中的 `DATABASE_URL` 配置是否正确

### 问题 3: CORS 错误

**解决方案**: 在 `config.py` 中添加你的前端 URL 到 `CORS_ORIGINS` 列表

### 问题 4: JWT 认证失败

**解决方案**: 确保请求头中包含正确的 Authorization token

## 项目结构详解

```
backend/
├── app/
│   ├── __init__.py              # Flask 应用工厂
│   ├── models/
│   │   ├── __init__.py
│   │   ├── user.py              # 用户和家庭组模型
│   │   ├── event.py             # 事件和提醒模型
│   │   └── role.py              # 角色和权限模型
│   ├── routes/
│   │   ├── __init__.py
│   │   ├── auth_bp.py           # 认证路由
│   │   ├── user_bp.py           # 用户管理路由
│   │   ├── family_bp.py         # 家庭组管理路由
│   │   ├── event_bp.py          # 事件管理路由
│   │   └── calendar_bp.py       # 日历查询路由
│   ├── services/                # 业务逻辑服务
│   └── utils/                   # 工具函数
├── config.py                    # 应用配置
├── run.py                       # 启动脚本
├── requirements.txt             # 依赖列表
└── .env.example                 # 环境变量示例
```

## 开发工作流

### 创建新的 API 端点

1. 在适当的 `routes/` 文件中创建路由
2. 在 `app/__init__.py` 中注册蓝图
3. 在 `docs/API.md` 中文档化端点
4. 编写测试（待建立）

### 修改数据库模型

1. 在 `models/` 中修改模型
2. 运行数据库迁移（使用 Alembic，待集成）
3. 更新相关的路由和服务

### 测试更改

```bash
# 在虚拟环境中运行测试
pytest tests/  # 待建立测试套件
```

## 生产部署

见 `docs/DEPLOYMENT.md` (待编写)

## 更多帮助

- 查看 `docs/API.md` 了解 API 详情
- 查看 `README.md` 了解项目概述
