---
name: code-scaffold
description: 根据技术架构设计文档生成项目脚手架代码。支持任意技术栈，根据架构文档中确定的语言和框架生成对应的项目骨架，包含目录结构、基础配置、公共组件、示例CRUD模块。当用户提到"生成代码"、"脚手架"、"scaffold"、"初始化项目"、"项目骨架"、"代码生成"时激活。
---

# 代码脚手架生成 Skill

根据架构设计文档生成完整的项目脚手架代码。支持任意技术栈。

## 触发场景

- 用户要求生成项目代码骨架
- 用户说"帮我初始化项目"、"生成脚手架"
- 技术架构确认后进入编码阶段

## 前置条件

- 需要有技术架构设计文档（明确了技术选型）
- 如果没有，先引导用户使用 architecture-design skill

## 执行流程

### Phase 1: 读取架构文档

1. 读取 `docs/architecture/[技术架构]xxx.md`
2. 提取技术选型信息（语言、框架、数据库、包管理器）
3. 确认项目名称和包路径/模块名

### Phase 2: 搜索参考模板

根据架构文档中的技术栈搜索社区最佳实践：
- `npx skills find {框架名} best-practices`
- 联网搜索 "{框架} project structure best practice 2026"
- 搜索 "{框架} starter template github"

### Phase 3: 生成项目骨架

**根据架构文档中确定的技术栈生成**，以下是常见技术栈的目录规范参考：

#### Java + Spring Boot + Maven
```
src/main/java/com/{group}/{artifact}/
├── Application.java
├── config/ | common/ | controller/ | service/impl/ | mapper/ | entity/ | dto/
src/main/resources/
├── application.yml | application-dev.yml | mapper/
```

#### Python + FastAPI
```
app/
├── __init__.py | config.py | database.py
├── models/ | schemas/ | api/v1/ | services/ | repositories/ | core/
main.py | alembic/ | tests/
```

#### Go + Gin
```
cmd/server/main.go
internal/
├── config/ | handler/ | service/ | repository/ | model/ | middleware/
pkg/ | api/ | migrations/ | configs/
```

#### C# + ASP.NET Core
```
src/{ProjectName}/
├── Controllers/ | Services/ | Models/ | Data/ | DTOs/ | Middleware/
├── Program.cs | appsettings.json
tests/{ProjectName}.Tests/
```

#### Node.js + NestJS
```
src/
├── main.ts | app.module.ts
├── {module}/
│   ├── {module}.controller.ts | {module}.service.ts | {module}.module.ts
│   ├── dto/ | entities/
prisma/ | test/
```

#### Rust + Actix-web
```
src/
├── main.rs | config.rs | db.rs | errors.rs
├── handlers/ | models/ | services/ | middleware/
migrations/ | tests/
```

**注意：以上仅为参考，实际结构以联网搜索到的最新最佳实践为准。**

### Phase 4: 生成公共组件

根据所选技术栈生成通用组件：
- 统一响应格式
- 全局异常处理
- 日志配置
- 数据库连接配置
- 认证/鉴权中间件（如需要）

### Phase 5: 生成示例 CRUD 模块（可选）

根据需求文档中的第一个业务模块生成完整示例。

### Phase 6: 生成辅助文件

- README.md（启动步骤）
- .gitignore（根据语言生成）
- Docker 相关（可选）
- 数据库初始化脚本
- 环境变量示例文件

## 关键原则

- **不固化语言**：严格按架构文档中的技术选型生成，不要默认用 Java
- **遵循语言惯例**：Go 用 Go 的命名规范，Python 用 PEP 8，C# 用 .NET 规范
- **包管理器正确**：Maven/Gradle/npm/pnpm/pip/go mod/cargo/dotnet — 根据语言选择
- **版本最新**：依赖版本联网确认最新稳定版
- **可直接运行**：生成的代码必须能编译通过并启动

## 输出规则

- 代码注释使用中文
- 配置文件敏感信息使用占位符（如 `${DB_PASSWORD}`）
- 生成的代码必须可直接编译/运行
- 遵循所选技术栈的社区命名规范


## V1.1 编码前置条件

开始生成或修改项目骨架前，必须读取：

```text
docs/PRD.md
docs/DESIGN.md          # 存在时
docs/ARCHITECTURE.md
docs/TODO.md
```

若这些文件缺失、互相冲突或与现有代码冲突，先停止并请求澄清。生成结果必须遵守它们的范围、视觉和技术边界。

脚手架完成后，除原有要求外还必须：
- 提供健康检查、基础配置读取、日志和密钥占位符。
- 有 UI 时实现 DESIGN 规定的基础组件和关键状态。
- 生成 `.env.example` 与最小启动/测试说明。
- 执行编译或启动级冒烟测试，失败时先修复。
