# MySQL 核心知识学习脉络

## 一、MySQL 基础入门
### 数据库与表结构
- **数据库概念**
    - 数据库定义：长期存储在计算机内、有组织、可共享的数据集合
    - 数据库管理系统（DBMS）：MySQL 作为开源的关系型数据库管理系统，用于创建、管理和维护数据库
- **表结构设计**
    - 表的概念：数据库中数据存储的基本单位，由行（记录）和列（字段）组成
    - 数据类型
        - 数值类型：整数（TINYINT、SMALLINT、INT 等）、小数（DECIMAL）
        - 字符串类型：CHAR、VARCHAR、TEXT
        - 日期和时间类型：DATE、TIME、DATETIME、TIMESTAMP
    - 约束
        - 主键约束（PRIMARY KEY）：唯一标识表中每一行数据，不能为空且唯一
        - 外键约束（FOREIGN KEY）：用于建立表与表之间的关联关系
        - 唯一约束（UNIQUE）：确保字段值的唯一性，但允许为空
        - 非空约束（NOT NULL）：规定字段值不能为空

### SQL 基础语句
- **数据定义语言（DDL）**
    - CREATE：创建数据库（CREATE DATABASE）、创建表（CREATE TABLE）
    - ALTER：修改表结构（ALTER TABLE），如添加列、修改列数据类型
    - DROP：删除数据库（DROP DATABASE）、删除表（DROP TABLE）
- **数据操作语言（DML）**
    - INSERT：插入数据到表中（INSERT INTO...VALUES，INSERT INTO...SELECT）
    - UPDATE：更新表中的数据（UPDATE...SET...WHERE）
    - DELETE：从表中删除数据（DELETE FROM...WHERE）
- **数据查询语言（DQL）**
    - SELECT：基本查询（SELECT * FROM...）
    - 条件查询（WHERE 子句）：如 SELECT * FROM table WHERE column = 'value'
    - 排序（ORDER BY 子句）：升序（ASC）、降序（DESC），如 SELECT * FROM table ORDER BY column DESC
    - 聚合函数（SUM、AVG、COUNT、MAX、MIN）：用于对数据进行统计计算，如 SELECT COUNT(*) FROM table
    - 分组（GROUP BY 子句）：结合聚合函数使用，如 SELECT column, COUNT(*) FROM table GROUP BY column

## 二、MySQL 高级查询
### 多表查询
- **连接查询**
    - 内连接（INNER JOIN）：返回两个表中满足连接条件的所有行，如 SELECT * FROM table1 INNER JOIN table2 ON table1.id = table2.id
    - 外连接
        - 左外连接（LEFT JOIN）：返回左表中的所有行以及右表中满足连接条件的行
        - 右外连接（RIGHT JOIN）：返回右表中的所有行以及左表中满足连接条件的行
        - 全外连接（FULL OUTER JOIN）：返回两个表中的所有行，匹配的行合并，不匹配的行对应列填 NULL（MySQL 需通过 UNION 模拟实现）
    - 交叉连接（CROSS JOIN）：返回两个表的笛卡尔积，即左表的每一行与右表的每一行都进行组合
- **子查询**
    - 嵌套子查询：子查询嵌套在主查询中，如 SELECT * FROM table1 WHERE column1 = (SELECT column2 FROM table2 WHERE condition)
    - 关联子查询：子查询引用主查询中的列，主查询的每一行都会执行一次子查询
    - 标量子查询：子查询只返回单个值，常用于比较条件中

### 视图与索引
- **视图（VIEW）**
    - 视图定义：虚拟表，基于 SQL 查询结果集，不实际存储数据
    - 创建视图（CREATE VIEW）：如 CREATE VIEW view_name AS SELECT * FROM table WHERE condition
    - 视图的作用：简化复杂查询、提供数据安全性，用户只能看到视图定义的数据
- **索引（INDEX）**
    - 索引类型
        - 普通索引（CREATE INDEX）：最基本的索引类型，加快查询速度
        - 唯一索引（CREATE UNIQUE INDEX）：确保索引列值的唯一性
        - 主键索引（PRIMARY KEY）：特殊的唯一索引，一个表只能有一个主键索引
        - 组合索引（CREATE INDEX...ON (col1, col2,...)）：基于多个列创建的索引，遵循最左前缀原则
    - 索引原理：基于数据结构（如 B - Tree、哈希表）实现，加速数据查找
    - 索引优化：避免索引失效（函数操作、数据类型不匹配等情况），合理使用覆盖索引

## 三、MySQL 数据库管理
### 用户管理与权限控制
- **用户管理**
    - 创建用户（CREATE USER）：如 CREATE USER 'username'@'host' IDENTIFIED BY 'password'
    - 修改用户密码（ALTER USER）：ALTER USER 'username'@'host' IDENTIFIED BY 'new_password'
    - 删除用户（DROP USER）：DROP USER 'username'@'host'
- **权限管理**
    - 权限类型：SELECT、INSERT、UPDATE、DELETE、CREATE、DROP 等
    - 授予权限（GRANT）：GRANT privilege_type ON database_name.table_name TO 'username'@'host'
    - 撤销权限（REVOKE）：REVOKE privilege_type ON database_name.table_name FROM 'username'@'host'

### 数据库备份与恢复
- **逻辑备份与恢复**
    - mysqldump 工具：用于备份数据库结构和数据，如 mysqldump -u username -p database_name > backup.sql
    - 恢复备份：使用 mysql 命令恢复，如 mysql -u username -p < backup.sql
- **物理备份与恢复**
    - 冷备份：在数据库关闭状态下复制数据文件，适用于所有存储引擎
    - 热备份：在数据库运行时进行备份，对于 InnoDB 引擎可使用 XtraBackup 工具

### 性能优化
- **查询性能优化**
    - EXPLAIN 关键字：分析查询语句的执行计划，查看索引使用情况、表连接顺序等
    - 慢查询日志：开启慢查询日志记录执行时间超过指定阈值的查询，分析优化慢查询语句
    - 优化器提示（Optimizer Hints）：给查询优化器提供建议，如 USE INDEX、FORCE INDEX 等
- **服务器性能优化**
    - 配置参数优化：调整 MySQL 配置文件（my.cnf 或 my.ini）中的参数，如 buffer_pool_size、innodb_log_file_size 等
    - 硬件优化：合理配置服务器硬件资源，如内存、CPU、磁盘 I/O 等

## 四、MySQL 存储引擎
### 存储引擎概述
- **存储引擎概念**：负责数据的存储和检索，MySQL 可支持多种存储引擎，不同引擎有不同特点和适用场景
- **查看与选择存储引擎**
    - 查看当前支持的存储引擎：SHOW ENGINES
    - 创建表时指定存储引擎：CREATE TABLE table_name (column_definitions) ENGINE = engine_type

### 常见存储引擎
- **InnoDB**
    - 特点：支持事务（ACID 特性）、行级锁、外键约束，适合处理大量并发事务的应用场景
    - 事务处理：BEGIN（或 START TRANSACTION）开始事务，COMMIT 提交事务，ROLLBACK 回滚事务
    - 锁机制：行级锁提高并发性能，减少锁争用
- **MyISAM**
    - 特点：不支持事务、表级锁，查询性能较高，但在写入操作时会锁定整个表，适用于读多写少的场景
    - 表结构与数据文件：分别存储为.MYI（索引文件）和.MYD（数据文件）
- **Memory**
    - 特点：数据存储在内存中，读写速度极快，但服务器重启数据丢失，适用于临时数据存储或缓存场景
    - 数据结构：使用哈希表或树作为索引结构

## 五、MySQL 高级特性
### 事务与锁机制
- **事务特性（ACID）**
    - 原子性（Atomicity）：事务中的操作要么全部成功，要么全部失败回滚
    - 一致性（Consistency）：事务执行前后数据库的完整性约束保持不变
    - 隔离性（Isolation）：不同事务之间相互隔离，不会相互干扰
    - 持久性（Durability）：事务一旦提交，对数据的修改是永久性的
- **事务隔离级别**
    - 读未提交（READ - UNCOMMITTED）：可能出现脏读、不可重复读、幻读
    - 读已提交（READ - COMMITTED）：避免脏读，但仍可能出现不可重复读、幻读
    - 可重复读（REPEATABLE - READ）：MySQL 默认隔离级别，避免脏读、不可重复读，但可能出现幻读
    - 串行化（SERIALIZABLE）：最高隔离级别，避免所有并发问题，但性能较低
- **锁机制**
    - 共享锁（S 锁）：又称读锁，允许其他事务对同一数据进行读操作，但不允许写操作
    - 排他锁（X 锁）：又称写锁，不允许其他事务对同一数据进行读写操作
    - 意向锁：分为意向共享锁（IS）和意向排他锁（IX），用于表级锁和行级锁之间的协调

### 分区表与分库分表
- **分区表**
    - 分区概念：将一个大表按照某种规则分成多个较小的部分，每个部分称为一个分区
    - 分区类型
        - 范围分区（RANGE PARTITIONING）：按列值范围进行分区，如按日期范围分区
        - 列表分区（LIST PARTITIONING）：按列值列表进行分区
        - 哈希分区（HASH PARTITIONING）：通过对列值进行哈希运算来分区
        - 键分区（KEY PARTITIONING）：类似哈希分区，使用 MySQL 提供的哈希函数
    - 分区的优势：提高查询性能、便于管理大表数据、增强数据可用性
- **分库分表**
    - 垂直分库：按照业务功能将不同模块的数据划分到不同的数据库中，降低单个数据库的压力
    - 垂直分表：将表中某些不常用或大字段拆分到另一个表，减少单表数据量
    - 水平分表：按照某种规则（如取模、按日期等）将数据均匀分布到多个表中，提高并发性能

### 主从复制与集群
- **主从复制**
    - 原理：主服务器将数据修改记录到二进制日志（binlog），从服务器通过 I/O 线程读取主服务器的 binlog 并写入中继日志（relay log），再由 SQL 线程将中继日志中的记录应用到从服务器，实现数据同步
    - 配置步骤：主服务器开启二进制日志、配置 server - id；从服务器配置 server - id，通过 CHANGE MASTER TO 命令连接主服务器
    - 作用：提高数据可用性、分担读压力、数据备份
- **集群**
    - MySQL Cluster：基于 NDB 存储引擎的分布式集群，提供高可用性、可扩展性和数据一致性
    - Galera Cluster：多主复制集群，所有节点都是主节点，数据同步采用同步复制方式，保证数据的强一致性
 