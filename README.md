# Project Dev Suite Router

一套用于 **AI 辅助软件项目开发** 的 Codex Skills 流程套件。

它不是一个默认参与所有任务的普通开发 Skill，而是一套面向软件项目全生命周期的流程化协作系统，适用于新项目启动、版本迭代、维护修复、上线前测试和高风险变更治理。

核心目标是让 AI 不只是“快速写代码”，而是能够在清晰上下文中，按照可验证、可回退、可持续迭代的工程流程协作。

---

## 1. 项目简介

`project-dev-suite-router` 是一个 Codex Skill 套件入口。

它会在用户明确需要进行项目级开发流程时启用，并根据任务复杂度选择不同执行强度，避免普通小任务被完整项目流程拖慢。

这套流程覆盖：

- 产品定位与 PRD
- UI 设计契约与原型
- 架构设计
- 任务拆解与 TODO 管理
- 代码骨架与冒烟测试
- 小步迭代开发
- 常规测试
- 发布前风险驱动测试
- 安全攻防测试
- 发布就绪检查
- 版本迭代
- 维护与 Hotfix

---

## 2. 核心理念

这套 Skills 的核心理念是：

```text
先判断任务有多重，再决定流程走多深、上下文读多少、测试做多深。
```

它不会要求每个小任务都执行完整流程，而是通过 `L0 / L1 / L2 / L3` 四档强度进行调度。

| 强度 | 名称 | 适用场景 |
|---|---|---|
| L0 | 不启用项目套件 | 普通问答、代码解释、单文件小修、小脚本 |
| L1 | 轻量上下文模式 | 已有项目中的小功能、小 bug、小优化 |
| L2 | 标准项目迭代模式 | 一个明确版本或一组有业务意义的功能 |
| L3 | 发布或高风险门禁模式 | 发布前、重大合并、高风险 hotfix、资金/权限/数据/安全变更 |

---

## 3. 适用场景

适合使用本套件的场景：

- 从 0 到 1 启动一个软件项目。
- 为已有项目规划 V1.1、V2.0 等版本迭代。
- 给已有项目新增一组功能。
- 准备上线发布。
- 处理安全、权限、数据、资金相关的高风险变更。
- 处理线上故障、性能退化、依赖风险或 hotfix。
- 希望 AI 按工程流程协作，而不是只做零散代码生成。

不适合完整启用的场景：

- 普通技术问答。
- 单文件小修改。
- 简单代码解释。
- 临时脚本。
- 非软件项目任务。

---

## 4. 目录结构

推荐仓库结构如下：

```text
project-dev-suite-router/
├─ README.md
├─ SKILL.md
├─ agents/
│  └─ openai.yaml
└─ references/
   ├─ workflow.md
   ├─ mcp/
   │  └─ mcp-template.json
   ├─ steering/
   │  ├─ global-steering.md
   │  └─ database-steering.md
   ├─ skills/
   │  ├─ requirements-analysis.md
   │  ├─ task-breakdown.md
   │  ├─ prototype-generator.md
   │  ├─ flowchart-generator.md
   │  ├─ architecture-design.md
   │  ├─ database-design.md
   │  ├─ requirements-transform.md
   │  ├─ detail-requirements.md
   │  ├─ api-doc-generator.md
   │  ├─ code-scaffold.md
   │  ├─ test-generation.md
   │  ├─ lint.md
   │  └─ find-bugs.md
   └─ templates/
      ├─ PRD.md
      ├─ DESIGN.md
      ├─ ARCHITECTURE.md
      ├─ TODO.md
      ├─ VERSION-PLAN.md
      ├─ MAINTENANCE-RECORD.md
      ├─ RELEASE-READINESS.md
      ├─ RISK-BASED-TEST-PLAN.md
      ├─ TEST-AUTOMATION-RUNBOOK.md
      ├─ TEST-EFFECTIVENESS-REPORT.md
      └─ SECURITY-ATTACK-REPORT.md
```

---

## 5. 安装方式

将整个 `project-dev-suite-router` 文件夹放到 Codex 的 skills 目录下。

常见位置示例：

```text
~/.codex/skills/project-dev-suite-router
```

如果你使用了自定义 `CODEX_HOME`，则放到：

```text
$CODEX_HOME/skills/project-dev-suite-router
```

安装后，Codex 会通过 `SKILL.md` 中的描述识别这个 Skill。

---

## 6. 使用方式

当你需要启动项目级流程时，可以这样说：

```text
我想做一个新的软件项目，请启用项目开发流程。
```

或者：

```text
我们要规划这个项目的 V1.1 版本，请按项目开发流程走。
```

或者：

```text
这个版本准备上线，请执行发布前风险测试和发布就绪检查。
```

Codex 应先判断：

```text
路径：新项目 / 版本迭代 / 维护热修复
强度：L1 / L2 / L3
阶段：需求 / 设计 / 架构 / 开发 / 测试 / 发布前 / 维护
```

然后再决定读取哪些文档、执行哪些步骤。

---

## 7. 工作流程概览

完整流程包括：

```text
触发判断
-> 项目状态识别
-> 产品定位与 PRD
-> 设计契约与原型
-> 架构设计
-> 两段式任务规划
-> 详细需求 / API / 数据库设计
-> 代码骨架与冒烟测试
-> 小步迭代开发
-> 常规测试
-> 发布前风险测试与安全攻防门禁
-> 发布就绪检查
-> 版本迭代 / 维护热修复
```

实际使用时，并不要求每次都从头执行完整流程。

例如：

- 小 bug：通常走 L1。
- 一个版本：通常走 L2。
- 发布前：走 L3。
- 普通问答：走 L0，不启用项目套件。

---

## 8. 关键文档产物

| 文档 | 作用 |
|---|---|
| `docs/PRD.md` | 产品定位、MVP、非目标、范围、验收标准 |
| `docs/DESIGN.md` | UI 视觉、组件、交互、状态设计 |
| `docs/ARCHITECTURE.md` | 技术栈、模块边界、权限边界、数据边界、不可破坏规则 |
| `docs/TODO.md` | 当前可执行任务队列 |
| `docs/releases/{version}/VERSION-PLAN.md` | 版本目标、范围、影响分析、回归范围 |
| `docs/maintenance/MAINTENANCE-RECORD.md` | bug、hotfix、安全、性能、事故维护记录 |
| `docs/tests/RISK-BASED-TEST-PLAN.md` | 风险驱动测试计划 |
| `docs/tests/TEST-AUTOMATION-RUNBOOK.md` | 自动化测试运行手册 |
| `docs/tests/TEST-EFFECTIVENESS-REPORT.md` | 测试真实性验证报告 |
| `docs/tests/SECURITY-ATTACK-REPORT.md` | 安全攻防测试报告 |
| `RELEASE-READINESS.md` | 发布就绪检查清单 |

---

## 9. 测试与安全门禁

本套件强调风险驱动测试，而不是盲目追求全面覆盖。

优先深测：

- 资金
- 权限
- 核心数据
- 用户隐私
- 安全边界
- AI 成本与隐私

发布前 L3 门禁包括：

- 高风险功能识别
- 正常 / 边界 / 异常测试设计
- 自动化测试命令固化
- 测试真实性验证
- 安全攻防测试
- 问题清单与方案核对
- 修复后回归

安全攻防测试覆盖：

- 数据越权
- 功能越权
- 身份认证绕过
- 字段提权
- 恶意输入与注入
- 绕过前端限制
- 敏感信息泄露
- 资源滥用与防刷

---

## 10. 版本历史

| 版本 | 主题 | 核心变化 |
|---|---|---|
| v1.0 | 基线版本 | 建立独立项目开发流程基础版 |
| v1.1 | 项目事实源与发布准备 | 增加 PRD、DESIGN、ARCHITECTURE、TODO、RELEASE-READINESS |
| v1.2 | 持续版本迭代与维护治理 | 增加新项目、版本迭代、维护 hotfix 三条路径 |
| v1.3 | 发布前风险测试与安全门禁 | 增加风险测试、安全攻防、测试真实性验证和四个测试产物 |
| v1.3.1 | 上游风险标记闭环 | PRD、架构、TODO、版本计划、维护记录增加轻量风险输入 |
| v1.4 | 轻量推理原则 | 增加第一性原理和对抗式审查，但不新增流程阶段 |
| v1.4.1 | 测试门禁触发粒度修正 | 明确完整 L3 门禁不针对每个单独 TODO 重复执行 |
| v1.5 | 流程强度分级与上下文节省 | 增加 L0-L3 强度分级、上下文预算、状态识别、完成定义 |

当前推荐版本：`v1.5.0`

---

## 11. 设计原则

### 11.1 该轻则轻，该重则重

普通小任务不应该启动完整流程。

发布前、高风险变更则必须有足够严格的验证。

### 11.2 文档服务开发

PRD、DESIGN、ARCHITECTURE、TODO 不是形式主义。

它们应该帮助 AI：

- 理解上下文
- 控制修改范围
- 判断风险
- 验证结果

### 11.3 不只看接口成功

测试不能只看接口是否返回 200。

还要验证：

- 业务结果是否正确
- 数据变化是否正确
- 权限校验是否有效
- 异常输入是否被拦截

### 11.4 高风险必须有证据

涉及资金、权限、核心数据、安全边界的变更，需要留下：

- 测试计划
- 自动化命令
- 攻防证据
- 问题清单
- 修复方案
- 回归结果

---

## 12. 推荐使用方式

```text
普通小问题：L0
已有项目小改动：L1
一个版本或一组功能：L2
发布前 / 高风险变更：L3
```

如果正确使用，这套流程不会显著加重日常开发，反而能减少返工、上下文丢失、测试无效和发布风险。

如果错误地把每个小任务都当成 L3，就会明显增加流程负担和 token 消耗。

---

## 13. 适合谁使用

适合：

- 使用 Codex 参与软件项目开发的人。
- 希望 AI 不只是写代码，而是参与完整工程流程的人。
- 需要管理 PRD、架构、任务、测试、发布的人。
- 希望减少 AI 乱改、乱扩展、乱猜需求的人。
- 重视安全、权限、数据和发布质量的人。

---

## 14. License

如果你希望别人自由学习、复制、修改和使用，可以使用 MIT License。

---

## 15. 一句话总结

这套项目开发 Skills 的核心不是“让 AI 快点写代码”，而是：

```text
让 AI 在正确的上下文里，按正确的流程，做可验证、可回退、可持续迭代的软件开发。
```