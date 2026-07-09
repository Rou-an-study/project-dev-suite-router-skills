---
name: find-bugs
description: Find bugs, security vulnerabilities, and code quality issues in local branch changes. Use when asked to review changes, find bugs, security review, or audit code on the current branch.
---

# Find Bugs

Review changes on this branch for bugs, security vulnerabilities, and code quality issues.

## Phase 1: Complete Input Gathering

1. Get the FULL diff: `git diff master...HEAD`
2. If cannot get diff by git, read files that modified this time
3. If output is truncated, read each changed file individually until you have seen every changed line
4. List all files modified in this branch before proceeding

## Phase 2: Attack Surface Mapping

For each changed file, identify and list:

* All user inputs (request params, headers, body, URL components)
* All database queries
* All authentication/authorization checks
* All session/state operations
* All external calls
* All cryptographic operations

## Phase 3: Security Checklist (check EVERY item for EVERY file)

* [ ] **Injection**: SQL, command, template, header injection
* [ ] **XSS**: All outputs in templates properly escaped?
* [ ] **Authentication**: Auth checks on all protected operations?
* [ ] **Authorization/IDOR**: Access control verified, not just auth?
* [ ] **CSRF**: State-changing operations protected?
* [ ] **Race conditions**: TOCTOU in any read-then-write patterns?
* [ ] **Session**: Fixation, expiration, secure flags?
* [ ] **Cryptography**: Secure random, proper algorithms, no secrets in logs?
* [ ] **Information disclosure**: Error messages, logs, timing attacks?
* [ ] **DoS**: Unbounded operations, missing rate limits, resource exhaustion?
* [ ] **Business logic**: Edge cases, state machine violations, numeric overflow?

## Phase 4: Verification

For each potential issue:

* Check if it's already handled elsewhere in the changed code
* Search for existing tests covering the scenario
* Read surrounding context to verify the issue is real

## Phase 5: Pre-Conclusion Audit

Before finalizing, you MUST:

1. List every file you reviewed and confirm you read it completely
2. List every checklist item and note whether you found issues or confirmed it's clean
3. List any areas you could NOT fully verify and why
4. Only then provide your final findings

## Output Format

**Prioritize**: security vulnerabilities > bugs > code quality

**Skip**: stylistic/formatting issues

For each issue:

* **File:Line** - Brief description
* **Severity**: Critical/High/Medium/Low
* **Problem**: What's wrong
* **Evidence**: Why this is real (not already fixed, no existing test, etc.)
* **Fix**: Concrete suggestion
* **References**: OWASP, RFCs, or other standards if applicable

If you find nothing significant, say so - don't invent issues.

Do not make changes - just report findings. I'll decide what to address.

所有的输出需要中文回答


## V1.2 发布与热修复审查

在版本或 hotfix 打标签前，审查本次变更是否：

- 超出版本计划或维护记录规定的范围。
- 破坏接口、数据、权限、安全、性能或不可破坏规则。
- 缺少必要的回归测试、部署说明或回滚方案。
- 将真实密钥、调试配置或未完成实验带入发布版本。

发现范围外问题时，记录为后续 TODO，除非用户确认将其纳入本次版本。

## V1.3 安全攻防测试门禁

在发布前或高风险 hotfix 前，以专业测试与安全工程师视角执行。先审查后端代码，再在授权测试环境中尽量写出可运行脚本或请求实测接口。发现问题先列清单，不直接修代码。

### 攻击类型

逐条尝试并记录证据：

1. 数据越权：修改订单号、用户 ID、租户 ID 等，尝试访问、修改或删除他人数据。
2. 功能越权：普通用户调用管理员、审核员、财务等专用接口。
3. 身份认证：不登录、伪造 token、篡改 token、过期 token 调用接口。
4. 偷改字段提权：提交额外字段，如 `role=admin`、`price=0`、`amount=-100`、`isPaid=true`。
5. 恶意输入与注入：SQL 注入、命令注入、脚本注入、模板注入、超长输入。
6. 绕过前端限制：直接请求接口，验证后端是否独立校验。
7. 敏感信息泄露：检查返回中是否暴露密码、密钥、token、手机号、身份证、其他用户数据。
8. 资源滥用与防刷：登录、发短信、下单、导出、AI 调用等接口的高频、超大、并发请求风险；破坏性压测需用户明确授权测试环境。

### 输出要求

输出到 `docs/tests/SECURITY-ATTACK-REPORT.md`：

- 问题标题、风险等级、风险类型。
- 影响后果：资金、权限、数据、可用性或隐私。
- 复现步骤：请求方法、路径、关键参数、身份、返回摘要。
- 证据：返回内容、状态码、数据变化或日志摘要。
- 建议方案和潜在牵连。
- 用户核对状态。

在用户确认问题和方案前，不进行修复。修复后必须重跑相关攻防用例和回归测试。
## V1.4 对抗式审查边界

对抗式审查作为安全和质量审查的思考姿态，不新增独立阶段。审查时优先攻击高影响变化：资金、权限、数据、外部服务、可靠性和发布回滚。发现问题仍按现有问题清单、用户核对、修复回归流程执行。