# SQL ALTER TABLE 语句

## ALTER TABLE 语句
ALTER TABLE 语句用于在已有的表中添加、修改或删除列。

### SQL ALTER TABLE 语法
如需在表中添加列，请使用下列语法:

```sql
ALTER TABLE table_name
ADD column_name datatype
```
要删除表中的列，请使用下列语法：

```sql
ALTER TABLE table_name 
DROP COLUMN column_name
```
注释：某些数据库系统不允许这种在数据库表中删除列的方式 (DROP COLUMN column_name)。

要改变表中列的数据类型，请使用下列语法：

```sql
ALTER TABLE table_name
ALTER COLUMN column_name datatype
```

> Persons 表:


Id |	LastName	|FirstName	|Address	|City
---|-------------|------------|-----------|---
1	|Adams	|John	|Oxford Street	|London
2	|Bush	|George	|Fifth Avenue	New |York
3	|Carter	|Thomas	|Changan Street	|Beijing

- SQL ALTER TABLE 实例
	现在，我们希望在表 "Persons" 中添加一个名为 "Birthday" 的新列。
	
	我们使用下列 SQL 语句：
	
	```sql
	ALTER TABLE Persons
	ADD Birthday date
	```
	请注意，新列 "Birthday" 的类型是 date，可以存放日期。数据类型规定列中可以存放的数据的类型。
	
	> 新的 "Persons" 表类似这样：
	
	Id |	LastName	|FirstName	|Address	|City | Birthday
	---|-------------|------------|-----------|---|--------
	1	|Adams	|John	|Oxford Street	|London |
	2	|Bush	|George	|Fifth Avenue	New |York |
	3	|Carter	|Thomas	|Changan Street	|Beijing |
- 改变数据类型实例
	
	现在我们希望改变 "Persons" 表中 "Birthday" 列的数据类型。
	
	我们使用下列 SQL 语句：

	```sql
	ALTER TABLE Persons
	ALTER COLUMN Birthday year
	```
	请注意，"Birthday" 列的数据类型是 year，可以存放 2 位或 4 位格式的年份。

- DROP COLUMN 实例

	接下来，我们删除 "Person" 表中的 "Birthday" 列：

	```sql
	ALTER TABLE Person
	DROP COLUMN Birthday
	```
	
	> Persons 表会成为这样:

	Id |	LastName	|FirstName	|Address	|City
	---|-------------|------------|-----------|---
	1	|Adams	|John	|Oxford Street	|London
	2	|Bush	|George	|Fifth Avenue	New |York
	3	|Carter	|Thomas	|Changan Street	|Beijing