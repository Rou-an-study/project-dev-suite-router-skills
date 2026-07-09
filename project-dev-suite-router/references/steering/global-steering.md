---
inclusion: manual
---

# 全局开发规范

## 语言规范
- 所有对话、文档、代码注释使用中文
- 代码变量/函数/类名使用英文

## 项目文档目录规范
所有项目文档统一存放在 `docs/` 目录下：
```
docs/
├── requirements/     # 需求文档
├── prototypes/       # 原型页面
├── architecture/     # 架构设计
├── diagrams/         # 流程图
└── api/              # 接口文档
database/             # 数据库脚本（与docs同级）
```

## 技术栈选择
- **不固化语言**：每个项目的技术栈由用户在架构设计阶段确定
- **常用组合参考**（仅供参考，不强制）：
  - Java + Spring Boot + Maven + MyBatis-Plus
  - Python + FastAPI + SQLAlchemy
  - Go + Gin + GORM
  - C# + ASP.NET Core + EF Core
  - Node.js + NestJS + Prisma
  - Vue 3 + Element Plus（前端）
  - React + Ant Design（前端）
- **数据库**：MySQL / PostgreSQL / MongoDB / SQLite（按项目需求选择）
- **必须联网确认最新稳定版本号**

## 代码风格
- 遵循所选语言的社区主流规范
- Java：阿里巴巴Java开发手册
- Python：PEP 8 + type hints
- Go：gofmt + 官方 Effective Go
- C#：.NET 编码约定
- TypeScript/Vue：ESLint + Prettier

## MCP 工具
- **mysql-custom**：数据库操作（get_table_names / get_single_table_ddl / execute）
- **chrome-devtools**：浏览器调试、原型预览
- **drawio**：AI 生成 Draw.io 图表（复杂架构图、云架构图时使用）

## 工作流程
当用户开始一个新项目时，按以下顺序推进：
1. 需求分析（requirements-analysis）
2. 原型设计 + 流程图（prototype-generator + flowchart-generator）
3. 架构设计（architecture-design）— **此阶段确定技术栈**
4. 数据库设计（database-design）
5. 需求转化（requirements-transform）
6. 详细需求 + 接口文档（detail-requirements + api-doc-generator）
7. 代码生成（code-scaffold）
8. 迭代开发（lint + find-bugs）


## V1.1 核心文档与交付规范

启用项目开发套件后，`docs/PRD.md`、`docs/DESIGN.md`（有 UI 时）、`docs/ARCHITECTURE.md` 与 `docs/TODO.md` 是项目事实源。

- 实现前读取相关事实源；冲突时先请求决策。
- 先做产品边界和 MVP，再做原型、架构或代码。
- 采用两段式计划：路线图先行，细粒度 TODO 后置。
- 每个 TODO 写清目标、允许修改范围、不可破坏项、验收和测试。
- 发布前根据最终代码反扫架构、环境变量、部署方式、README、已知限制和回滚方式。


## V1.2 生命周期分流

启用套件后，先判断请求属于新项目、版本迭代还是维护/热修复。

- 新项目走产品定位到首发流程。
- 版本迭代先读取现有事实源，创建版本计划，完成影响分析与回归测试后发布。
- 维护/热修复走最小安全修复路径，并记录问题、影响、回归和复盘。

发布后更新 PRD、DESIGN、ARCHITECTURE、ROADMAP、README 和相关记录。不要将一次发布视为项目结束。

## V1.3 发布前测试门禁规范

风险驱动测试与安全攻防属于发布前质量门禁，不属于需求或架构阶段。PRD、DESIGN、ARCHITECTURE、TODO、VERSION-PLAN 和 MAINTENANCE-RECORD 只作为识别测试范围的输入。

发布、重大版本合并或高风险 hotfix 前，必须集中产出并核对：风险测试计划、自动化测试运行手册、测试有效性报告、安全攻防报告。资金、权限、数据相关功能深测；次要功能够用即可，避免低价值过度测试。
## V1.3.1 上游风险标记规范

上游文档只做轻量风险标记：PRD 标记功能是否涉及资金、权限、数据；ARCHITECTURE 说明权限、数据、接口和安全边界；TODO、VERSION-PLAN、MAINTENANCE-RECORD 标记任务或变更风险类型和回归范围。完整测试用例、攻防验证和问题核对仍集中在发布前测试门禁执行。
## V1.4 轻量推理规范

高影响决策使用第一性原理：先拆目标、约束、不可破坏规则，再参考成熟方案。对抗式审查只作为关键节点的检查姿态，不新增独立阶段或额外报告；已有风险测试与安全攻防门禁仍是主要执行点。
## V1.4.1 测试门禁触发粒度

完整 V1.3 风险测试与安全攻防门禁只在版本发布前、重大功能集合合并前或高风险 hotfix 发布前执行，不对每个单独 TODO 重复执行。单个 TODO 完成后只做相关常规测试和必要局部风险验证；涉及资金、权限、数据的单个高风险功能完成后，可提前做局部风险测试与必要安全验证。

## V1.5 流程强度与上下文预算

启用项目开发套件前先判断强度：L0 不启用、L1 轻量上下文、L2 标准迭代、L3 发布或高风险门禁。默认选择能安全完成任务的最低强度；只有当任务涉及版本发布、重大功能集合、高风险 hotfix、资金、权限、核心数据或安全边界时，才升级到 L3。读取上下文时先读最小必要事实源，发现冲突、缺口或高风险后再扩大范围。
