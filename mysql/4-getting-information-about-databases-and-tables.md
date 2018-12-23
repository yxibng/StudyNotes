1. 查看当前数据库

	```sql
	mysql> SELECT DATABASE();
	+------------+
	| DATABASE() |
	+------------+
	| menagerie  |
	+------------+
	```
	区别一下
	
	```sql
	mysql> show databases;
	+--------------------+
	| Database           |
	+--------------------+
	| information_schema |
	| menagerie          |
	| mysql              |
	| performance_schema |
	| sys                |
	+--------------------+
	```
2. 列举当前数据库中所有的表
	
	```sql
	mysql> show tables;
	+---------------------+
	| Tables_in_menagerie |
	+---------------------+
	| event               |
	| pet                 |
	+---------------------+
	```
3. 查看表结构定义

	```sql
	mysql> describe pet;
	+---------+-------------+------+-----+---------+-------+
	| Field   | Type        | Null | Key | Default | Extra |
	+---------+-------------+------+-----+---------+-------+
	| name    | varchar(20) | YES  |     | NULL    |       |
	| owner   | varchar(20) | YES  |     | NULL    |       |
	| species | varchar(20) | YES  |     | NULL    |       |
	| sex     | char(1)     | YES  |     | NULL    |       |
	| birth   | date        | YES  |     | NULL    |       |
	| death   | date        | YES  |     | NULL    |       |
	+---------+-------------+------+-----+---------+-------+
	```