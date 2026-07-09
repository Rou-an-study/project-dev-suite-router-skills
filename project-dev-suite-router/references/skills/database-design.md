---
name: database-design
description: 数据库详细设计与操作。包含表结构设计、SQL生成、索引优化、数据迁移脚本等。支持MySQL数据库。配合MySQL MCP可直接执行SQL。当用户提到"数据库设计"、"建表"、"DDL"、"SQL"、"表结构"、"数据库"时激活。
---

# 数据库设计 Skill

数据库详细设计、SQL 生成与操作规范。

## 触发场景

- 用户要求设计数据库表结构
- 用户说"帮我建表"、"写 DDL"
- 用户需要数据库迁移脚本
- 用户要求优化 SQL 或索引

## 数据库操作规范

### 查询规范
- 使用参数化查询，防止 SQL 注入
- SELECT 语句必须指定具体字段，禁止使用 SELECT *
- 大数据量查询必须分页（默认 pageSize=10）
- WHERE 条件中的字段必须有索引覆盖

### 命名规范
- 表名：小写下划线，如 `meeting_summary`
- 字段名：小写下划线，如 `create_time`
- 索引名：`idx_{表名}_{字段名}`，如 `idx_meeting_summary_meeting_id`
- 唯一索引：`uk_{表名}_{字段名}`

### ID 生成策略
- 主键使用 BIGINT 类型
- Java 项目使用雪花算法（应用层生成）
- Python 项目使用数据库自增或 UUID

### 通用字段（每张表必须包含）
```sql
id BIGINT NOT NULL AUTO_INCREMENT COMMENT '主键ID',
create_time DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
update_time DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
create_by VARCHAR(64) DEFAULT NULL COMMENT '创建人',
update_by VARCHAR(64) DEFAULT NULL COMMENT '更新人',
is_deleted TINYINT NOT NULL DEFAULT 0 COMMENT '逻辑删除 0-正常 1-删除',
PRIMARY KEY (id)
```

## 执行流程

### Phase 1: 需求分析

1. 从需求文档中提取数据实体和关系
2. 识别核心表和辅助表
3. 确认字段类型和约束

### Phase 2: 生成 DDL

```sql
-- ============================================
-- 表名: {table_name}
-- 说明: {表说明}
-- 创建时间: {日期}
-- ============================================
CREATE TABLE `{table_name}` (
    `id` BIGINT NOT NULL AUTO_INCREMENT COMMENT '主键ID',
    -- 业务字段
    `{field_name}` {TYPE} {NULL/NOT NULL} {DEFAULT} COMMENT '{说明}',
    -- 通用字段
    `create_time` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    `create_by` VARCHAR(64) DEFAULT NULL COMMENT '创建人',
    `update_by` VARCHAR(64) DEFAULT NULL COMMENT '更新人',
    `is_deleted` TINYINT NOT NULL DEFAULT 0 COMMENT '逻辑删除 0-正常 1-删除',
    PRIMARY KEY (`id`),
    -- 索引
    KEY `idx_{table}_{field}` (`{field}`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci COMMENT='{表注释}';
```

### Phase 3: 生成索引建议

根据业务查询场景推荐索引：
- 外键字段必须建索引
- 高频查询条件字段建索引
- 状态+时间组合索引（覆盖列表查询）
- 唯一业务字段建唯一索引

### Phase 4: 生成初始化数据（可选）

```sql
-- 初始化数据
INSERT INTO `{table_name}` ({fields}) VALUES
({values1}),
({values2});
```

## 配合 MySQL MCP 使用

当项目配置了 MySQL MCP 时，可以直接执行：
1. `get_table_names` — 查看现有表
2. `get_single_table_ddl` — 查看表结构
3. `execute` — 执行 DDL/DML

## 输出规则

- SQL 文件保存到 `database/` 目录
- 文件命名：`V{版本号}__{描述}.sql`（如 `V1__init_tables.sql`）
- 每条 DDL 语句前加注释说明
- 所有字段必须有 COMMENT
- 字符集统一 utf8mb4
