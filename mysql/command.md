1. `SHOW CREATE TABLE tbl_name` 展示创建表的语句声明

	```sql
	mysql> SHOW CREATE TABLE t\G
	*************************** 1. row ***************************
	       Table: t
	Create Table: CREATE TABLE `t` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `s` char(60) DEFAULT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4
	```
2. 