# START TRANSACTION, COMMIT, and ROLLBACK Syntax





```sql
START TRANSACTION
    [transaction_characteristic [, transaction_characteristic] ...]

transaction_characteristic: {
    WITH CONSISTENT SNAPSHOT
  | READ WRITE
  | READ ONLY
}

BEGIN [WORK]
COMMIT [WORK] [AND [NO] CHAIN] [[NO] RELEASE]
ROLLBACK [WORK] [AND [NO] CHAIN] [[NO] RELEASE]
SET autocommit = {0 | 1}
```

不可以回滚的操作:

- 创建或删除数据库
- 创建,删除,修改表结构



事务控制语句:

- `start transaction` 或 `begin` 开启一个新的事务
- `commit` 提交当前的事务,使变更持久化.
- `rollback` 回滚当前的事务,取消其修改
- `set autocommit=0|1` 控制当前会话的自动保存开关
- `savepoint identifier` savepoint允许在事务中创建一个保存点，一个事务中可以有多个savepoint
- `release savepoint identifier` 删除一个事务的保存点，当没有指定的保存点时，执行该语句会抛出一个异常
- `rollback to identifier` 把事务回滚到标记点
- `set transaction` 用来设置事务的隔离级别。InnoDB存储引擎提供事务的隔离级别有
	- READ UNCOMMITTED 
	- READ COMMITTED
	- REPEATABLE READ
	- SERIALIZABLE

autocommit 注意事项:

- mysql 默认打开了 autocommit开关,因此所有的更新操作都被立即保存到磁盘,因此变更无法回滚. </br>
- 显示关闭autocommit的方法是, `set autocommit = 0`.</br>
- 使用`START TRANSACTION `会隐式的关闭自动提交功能.

```sql
START TRANSACTION;
SELECT @A:=SUM(salary) FROM table1 WHERE type=1;
UPDATE table2 SET summary=@A WHERE type=1;
COMMIT;
```




