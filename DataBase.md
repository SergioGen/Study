DataBase

# MySQL数据库

## MySQL事务

MySQL事务主要用于处理操作量大，复杂度高的数据。

1. 在MySQL中只有使用了Innodb数据引擎的数据库或表才支持事务。
2. 事务处理可以用于维护数据库的完整性，保证成批的SQL语句要么全部执行，要么全不执行。
3. 事务用来关联insert,update,delete语句。

一般来说，事务必须要满足四个条件(ACID)，Atomicity(原子性)、Consistency(稳定性)、Isolation(隔离性)、Durability(可靠性)

- 原子性：一组事务，要么成功，要么撤回

- 稳定性：有非法数据(外键约束之类)，事务撤回

- 隔离性：事务独立运行，事务100%隔离。

- 可靠性：软硬件崩溃后，InnoDB数据表驱动会利用日志文件重构修改。可靠性和高速度不可兼得，innodb_flush_log_at_trx_commit选项，决定什么时候把事务保存到日志里。

## MySQL索引

