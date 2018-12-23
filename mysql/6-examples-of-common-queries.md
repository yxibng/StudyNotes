# Examples of Common Queries

创建并使用数据库test

```
create database test;
use test;
```

创建表shop

```sql
CREATE TABLE shop (
    article INT(4) UNSIGNED ZEROFILL DEFAULT '0000' NOT NULL,
    dealer  CHAR(20)                 DEFAULT ''     NOT NULL,
    price   DOUBLE(16,2)             DEFAULT '0.00' NOT NULL,
    PRIMARY KEY(article, dealer));
```

插入数据到shop表

```sql
INSERT INTO shop VALUES
    (1,'A',3.45),(1,'B',3.99),(2,'A',10.99),(3,'B',1.45),
    (3,'C',1.69),(3,'D',1.25),(4,'D',19.95);
```

显示表内容

```sql
mysql> select * from shop;
+---------+--------+-------+
| article | dealer | price |
+---------+--------+-------+
|    0001 | A      |  3.45 |
|    0001 | B      |  3.99 |
|    0002 | A      | 10.99 |
|    0003 | B      |  1.45 |
|    0003 | C      |  1.69 |
|    0003 | D      |  1.25 |
|    0004 | D      | 19.95 |
+---------+--------+-------+
```

1. 查询列最大值

	```sql
	mysql> select max(article) as article from shop;
	+---------+
	| article |
	+---------+
	|       4 |
	+---------+
	```
2. 找出列最大值所对应的那条记录

	```sql
	mysql> select article, dealer, price from shop where price = (select max(price) from shop);
	+---------+--------+-------+
	| article | dealer | price |
	+---------+--------+-------+
	|    0004 | D      | 19.95 |
	+---------+--------+-------+
	```
3. 找出分组中的最大值

	```sql
	mysql> select article, max(price) as price from shop group by article;
	+---------+-------+
	| article | price |
	+---------+-------+
	|    0001 |  3.99 |
	|    0002 | 10.99 |
	|    0003 |  1.69 |
	|    0004 | 19.95 |
	+---------+-------+
	```
4. 找出分组中最大值所对应的记录
	
	关联子查询
	
	```sql
	mysql> select article, dealer, price from shop s1 where price=(select max(s2.price) from shop s2 where s1.article = s2.article );
	+---------+--------+-------+
	| article | dealer | price |
	+---------+--------+-------+
	|    0001 | B      |  3.99 |
	|    0002 | A      | 10.99 |
	|    0003 | C      |  1.69 |
	|    0004 | D      | 19.95 |
	+---------+--------+-------+
	```
	
	不关联子查询
	
	```sql
	mysql> SELECT s1.article, dealer, s1.price
    -> FROM shop s1
    -> JOIN (
    ->   SELECT article, MAX(price) AS price
    ->   FROM shop
    ->   GROUP BY article) AS s2
    ->   ON s1.article = s2.article AND s1.price = s2.price;
	+---------+--------+-------+
	| article | dealer | price |
	+---------+--------+-------+
	|    0001 | B      |  3.99 |
	|    0002 | A      | 10.99 |
	|    0003 | C      |  1.69 |
	|    0004 | D      | 19.95 |
	+---------+--------+-------+
	```
	左连接
	
	```sql
	mysql> SELECT s1.article, s1.dealer, s1.price
    -> FROM shop s1
    -> LEFT JOIN shop s2 ON s1.article = s2.article AND s1.price < s2.price
    -> WHERE s2.article IS NULL;
	+---------+--------+-------+
	| article | dealer | price |
	+---------+--------+-------+
	|    0001 | B      |  3.99 |
	|    0002 | A      | 10.99 |
	|    0003 | C      |  1.69 |
	|    0004 | D      | 19.95 |
	+---------+--------+-------+
	```
	


5. 使用自定义变量

	```sql
	mysql> SELECT @min_price:=MIN(price),@max_price:=MAX(price) FROM shop;
	mysql> SELECT * FROM shop WHERE price=@min_price OR price=@max_price;
	+---------+--------+-------+
	| article | dealer | price |
	+---------+--------+-------+
	|    0003 | D      |  1.25 |
	|    0004 | D      | 19.95 |
	+---------+--------+-------+
	```

6. 使用外键

	> persons
	
	```sql
	CREATE TABLE `Persons` (
	  `Id_P` int(11) NOT NULL AUTO_INCREMENT,
	  `LastName` varchar(16) NOT NULL,
	  `FirstName` varchar(16) NOT NULL,
	  `Address` varchar(16) DEFAULT NULL,
	  `City` varchar(16) DEFAULT NULL,
	  PRIMARY KEY (`Id_P`)
	)
	```
	
	> orders
	
	```sql
	CREATE TABLE `Orders` (
	  `Id_O` int(11) NOT NULL,
	  `OrderNo` int(11) NOT NULL,
	  `Id_P` int(11) DEFAULT NULL,
	  PRIMARY KEY (`Id_O`),
	  KEY `Id_P` (`Id_P`),
	  CONSTRAINT `orders_ibfk_1` FOREIGN KEY (`Id_P`) REFERENCES `persons` (`id_p`)
	)
	```
	
	数据
	
	```sql
	mysql> select * from persons;
	+------+----------+-----------+----------------+----------+
	| Id_P | LastName | FirstName | Address        | City     |
	+------+----------+-----------+----------------+----------+
	|    1 | Adams    | John      | Oxford Street  | London   |
	|    2 | Bush     | George    | Fifth Avenue   | New York |
	|    3 | Carter   | Thomas    | Changan Street | Beijing  |
	+------+----------+-----------+----------------+----------+
	
	mysql> select * from orders;
	+------+---------+------+
	| Id_O | OrderNo | Id_P |
	+------+---------+------+
	|    1 |   77895 |    3 |
	|    2 |   44678 |    3 |
	|    3 |   22456 |    1 |
	|    4 |   24562 |    1 |
	+------+---------+------+
	```
	连接查询
	
	```
	mysql> select * from Orders o inner join Persons p  on o.id_p=p.id_p;
	+------+---------+------+------+----------+-----------+----------------+---------+
	| Id_O | OrderNo | Id_P | Id_P | LastName | FirstName | Address        | City    |
	+------+---------+------+------+----------+-----------+----------------+---------+
	|    1 |   77895 |    3 |    3 | Carter   | Thomas    | Changan Street | Beijing |
	|    2 |   44678 |    3 |    3 | Carter   | Thomas    | Changan Street | Beijing |
	|    3 |   22456 |    1 |    1 | Adams    | John      | Oxford Street  | London  |
	|    4 |   24562 |    1 |    1 | Adams    | John      | Oxford Street  | London  |
	+------+---------+------+------+----------+-----------+----------------+---------+
	```

7. union 操作符
	
	==SQL UNION 语法==

	```sql
	SELECT column_name(s) FROM table_name1
	UNION
	SELECT column_name(s) FROM table_name2
	```
	注释: 默认地,UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。
	
	==SQL UNION ALL 语法==
	
	```sql
	SELECT column_name(s) FROM table_name1
	UNION ALL
	SELECT column_name(s) FROM table_name2
	```
	另外，UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名

	示例
	
	```sql
	mysql> select lastname from persons union select id_p from orders;
	+----------+
	| lastname |
	+----------+
	| Adams    |
	| Bush     |
	| Carter   |
	| 1        |
	| 3        |
	+----------+
	```
8. 	 AUTO_INCREMENT
	
	```sql
	CREATE TABLE animals (
	     id MEDIUMINT NOT NULL AUTO_INCREMENT,
	     name CHAR(30) NOT NULL,
	     PRIMARY KEY (id)
	);
	
	INSERT INTO animals (name) VALUES
	    ('dog'),('cat'),('penguin'),
	    ('lax'),('whale'),('ostrich');
	
	SELECT * FROM animals;
	+----+---------+
	| id | name    |
	+----+---------+
	|  1 | dog     |
	|  2 | cat     |
	|  3 | penguin |
	|  4 | lax     |
	|  5 | whale   |
	|  6 | ostrich |
	+----+---------+
	```
	插入一条数据,id从0开始,如果打开了==NO\_AUTO\_VALUE\_ON\_ZERO== ,id不能从0开始.

	```sql
	INSERT INTO animals (id,name) VALUES(0,'groundhog');
	```
	
	id被指定为not null,但是依然可以指定为null, 此时id 会根据上次的id++
	
	```sql
	INSERT INTO animals (id,name) VALUES(NULL,'squirrel');
	```
	
	可以指定id,下个记录会从新的id开始++
	
	```sql
	INSERT INTO animals (id,name) VALUES(100,'rabbit');
	INSERT INTO animals (id,name) VALUES(NULL,'mouse');
	SELECT * FROM animals;
	+-----+-----------+
	| id  | name      |
	+-----+-----------+
	|   1 | dog       |
	|   2 | cat       |
	|   3 | penguin   |
	|   4 | lax       |
	|   5 | whale     |
	|   6 | ostrich   |
	|   7 | groundhog |
	|   8 | squirrel  |
	| 100 | rabbit    |
	| 101 | mouse     |
	+-----+-----------+
	```


