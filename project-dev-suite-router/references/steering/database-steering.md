---
inclusion: fileMatch
fileMatchPattern: "**/*.sql,**/database/**,**/mapper/**,**/dao/**,**/repository/**"
---

# 数据库操作规范

## 查询规范
- 使用参数化查询，防止 SQL 注入
- SELECT 语句必须指定具体字段，禁止使用 SELECT *
- 大数据量查询必须分页
- WHERE 条件字段应有索引覆盖

## 表设计规范
- 表名使用小写下划线命名
- 每张表必须有 id、create_time、update_time、is_deleted 字段
- 字符集统一 utf8mb4_unicode_ci
- 所有字段必须有 COMMENT 注释

## ID 生成
- Java 项目：雪花算法（应用层生成，BIGINT 类型）
- Python 项目：数据库自增或 UUID

## 索引规范
- 外键字段必须建索引
- 索引命名：idx_{表名}_{字段名}
- 唯一索引命名：uk_{表名}_{字段名}
- 避免在低基数字段上建索引

## MySQL MCP 工具
如果配置了 mysql-custom MCP，可使用：
- `get_table_names` — 获取所有表名
- `get_single_table_ddl` — 获取表 DDL
- `execute` — 执行 SQL（支持雪花ID: use_snowflake_id=true）
