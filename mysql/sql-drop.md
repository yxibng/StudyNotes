# SQL 撤销索引、表以及数据库
通过使用 DROP 语句，可以轻松地删除索引、表和数据库。

- SQL DROP INDEX 语句
 我们可以使用 DROP INDEX 命令删除表格中的索引

 ```sql 
 ALTER TABLE table_name DROP INDEX index_name
 ```

- SQL DROP TABLE 语句
	
	DROP TABLE 语句用于删除表（表的结构、属性以及索引也会被删除）：
	
	```sql
	DROP TABLE 表名称
	```

- SQL DROP DATABASE 语句

	DROP DATABASE 语句用于删除数据库：

	```sql
	DROP DATABASE 数据库名称
	```

- SQL TRUNCATE TABLE 语句
	
	如果我们仅仅需要除去表内的数据，但并不删除表本身，那么我们该如何做呢？
	
	请使用 TRUNCATE TABLE 命令（仅仅删除表格中的数据）：

	```sql
	TRUNCATE TABLE 表名称
	```