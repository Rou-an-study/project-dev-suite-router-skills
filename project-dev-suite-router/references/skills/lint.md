---
name: lint
description: 项目代码检查与格式化。支持Java（checkstyle/spotbugs）、Vue/TypeScript（eslint/prettier）、Python（ruff/mypy）三种技术栈的lint和typecheck。当用户提到"lint"、"代码检查"、"格式化"、"typecheck"、"代码规范"时激活。
---

# Lint Skill

项目代码检查与格式化，支持多技术栈。

## 自动识别技术栈

根据项目根目录文件自动判断：
- 存在 `pom.xml` → Java/Maven 项目
- 存在 `package.json` → Node/Vue 前端项目
- 存在 `pyproject.toml` 或 `requirements.txt` → Python 项目

## Java / Maven 项目

### Lint 命令
```bash
# 编译检查（最基础的lint）
mvn compile -q

# 使用 checkstyle 检查代码规范
mvn checkstyle:check

# 使用 spotbugs 检查潜在bug
mvn spotbugs:check
```

### 常见问题修复
- 未使用的 import → IDE 自动清理或手动删除
- 命名不规范 → 类名大驼峰，方法/变量小驼峰，常量全大写下划线
- 缺少 Javadoc → 公共方法添加注释

## Vue / TypeScript 前端项目

### Lint 命令
```bash
# ESLint 检查
npx eslint src/ --ext .vue,.js,.ts,.tsx

# ESLint 自动修复
npx eslint src/ --ext .vue,.js,.ts,.tsx --fix

# Prettier 格式化
npx prettier --write "src/**/*.{vue,js,ts,css,scss}"

# TypeScript 类型检查
npx vue-tsc --noEmit
```

### 常见问题修复
- import 顺序 → 运行 `--fix` 自动排序
- 未使用变量 → 删除或添加 `_` 前缀
- 类型错误 → 检查类型定义是否匹配

## Python 项目

### Lint 命令
```bash
# Ruff 检查（替代 flake8 + isort + pyupgrade）
ruff check .

# Ruff 自动修复
ruff check . --fix

# Ruff 格式化（替代 black）
ruff format .

# Mypy 类型检查
mypy .
```

### 常见问题修复
- import 顺序 → `ruff check --fix` 自动排序
- 未使用变量 → 删除或添加 `_` 前缀
- 类型注解缺失 → 为公共函数添加类型注解

## 执行策略

1. 先运行 lint 检查，查看所有问题
2. 尝试自动修复（`--fix`）
3. 手动修复无法自动处理的问题
4. 再次运行确认全部通过
5. 运行 typecheck 确认类型安全

## 输出格式

报告格式：
```
✅ Lint 通过 - 无问题
或
❌ 发现 N 个问题：
  - {文件}:{行号} - {问题描述} [{规则名}]
  - ...
  
🔧 已自动修复 M 个问题
⚠️ 需手动修复 K 个问题
```
