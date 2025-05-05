# csharp-learning-notes
当然可以，下面是为你的项目量身定制的 **README 文件模板**，适用于你的最小 C# 登录注册图书管理系统（前后端分离，Podman 部署，MySQL + Redis 支持）：

---

````markdown
# 📚 LibraryManagement 登录注册图书管理系统

这是一个基于 **C# (.NET 6)** 和 **Vue 3** 开发的最小登录注册图书管理项目，支持用户注册登录、图书管理、借阅记录，使用 **MySQL + Redis** 作为后端服务，并通过 **Podman** 实现容器化部署，支持未来迁移到 Kubernetes。

---

## 📦 技术栈

| 组件       | 技术栈                        |
|------------|-------------------------------|
| 前端       | Vue 3 + Vite                  |
| 后端       | ASP.NET 6 Web API             |
| 数据库     | MySQL 8                       |
| 缓存       | Redis 7                       |
| 容器引擎   | Podman                        |
| 模块化结构 | `sln` + 多项目引用            |

---

## 🧠 项目结构

```bash
LibraryManagement/
├── LibraryManagement.sln       # .NET 解决方案
├── Api/                        # Web API 控制器与入口
├── Core/                       # 实体类与接口定义
├── Infrastructure/             # EF Core 上下文、Redis
├── Services/                   # 登录注册等业务逻辑
└── library-frontend/           # Vue 3 前端工程
````

---

## 🗄️ 数据模型

* **User**：用户名、密码、邮箱
* **Book**：书名、作者、ISBN
* **BorrowRecord**：用户借书与归还时间

数据库表结构已在 `docs/db-schema.sql` 中定义。

---

## 🚀 快速启动（基于 Podman）

### 1. 拉取基础镜像

```bash
podman pull mcr.microsoft.com/dotnet/sdk:6.0
podman pull mysql:8.0
podman pull redis:7
podman pull node:20
```

### 2. 创建 Pod（共享网络）

```bash
podman pod create --name lib-pod -p 8080:80 -p 5173:5173
```

### 3. 启动后端服务（API）

```bash
podman run -dt --pod lib-pod --name lib-backend \
  -v $(pwd)/LibraryManagement:/app \
  -w /app/Api \
  mcr.microsoft.com/dotnet/sdk:6.0 \
  bash -c "dotnet run"
```

### 4. 启动数据库服务

```bash
podman run -dt --pod lib-pod --name mysql \
  -e MYSQL_ROOT_PASSWORD=root \
  -e MYSQL_DATABASE=library \
  mysql:8.0
```

### 5. 启动 Redis 缓存

```bash
podman run -dt --pod lib-pod --name redis redis:7
```

### 6. 启动前端服务（Vue 3）

```bash
podman run -dt --pod lib-pod --name vue-frontend \
  -v $(pwd)/library-frontend:/app \
  -w /app \
  node:20 \
  bash -c "npm install && npm run dev -- --host"
```

> Vue 默认运行在 `http://localhost:5173`，后端 API 运行在 `http://localhost:8080`

---

## 🧪 功能列表

* [x] 用户注册
* [x] 用户登录（含密码加密验证）
* [x] 图书添加、查询、删除
* [x] 借阅记录管理
* [ ] Redis 登录状态缓存（进行中）
* [ ] 单元测试覆盖（待完善）

---

## 🧰 数据库初始化

如需手动创建数据库结构：

```bash
dotnet ef migrations add Init --project Infrastructure --startup-project Api
dotnet ef database update --project Infrastructure --startup-project Api
```

---

## ☸️ 可选：Kubernetes 部署（准备中）

后续将提供：

* `deployment.yaml`（后端）
* `mysql-deployment.yaml`
* `redis-deployment.yaml`
* `frontend-deployment.yaml`

---

## 📄 License

本项目仅用于学习与研究目的，开源协议遵循 MIT License。

---

```

---

这个 README 文件覆盖了你的项目结构、技术栈、使用方法、容器部署、未来扩展方向（K8s）等要素。如果你希望加入中文版本，或者需要将 README 拆分为多个文档（例如 `docs/` 文件夹），我可以继续补充。

你希望我接下来帮你补充哪一部分？（例如：生成 Vue 前端骨架、自动部署脚本、Redis 登录态管理等）
```

