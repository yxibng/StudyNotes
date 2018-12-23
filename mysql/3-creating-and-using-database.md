## 创建并使用数据库
 1. 显示所有的数据库,没有权限访问的不显示`show databases`

	```sql
	mysql> show databases;
	+--------------------+
	| Database           |
	+--------------------+
	| information_schema |
	| mysql              |
	| performance_schema |
	| sys                |
	+--------------------+
	4 rows in set (0.00 sec)
	```
2. 使用数据库,`use mysql`

	```sql
	mysql> use mysql;
	Database changed
	```
3. 授予权限

	```sql
	mysql> GRANT ALL ON menagerie.* TO 'your_mysql_name'@'your_client_host';
	```
	
	your_mysql_name  : mysql username
	your_client_host : the host from which you connect to the server
	
4. 创建数据库

	```sql
	mysql> CREATE DATABASE menagerie;
	mysql> USE menagerie
	Database changed
	```
	从别的机器访问数据库
	
	```sql
	shell> mysql -h host -u user -p menagerie
	Enter password: *****
	```

	> menagerie是数据库的名字,如果想指定密码在一个查询里可以用
	
	```sql
	shell> mysql -h host -u user -ppassword menagerie
	```
	不能是

	```	sql
	shell> mysql -h host -u user -p password menagerie
	
	```

## 创建表
1. 显示所有的表
	
	```sql
	mysql> SHOW TABLES;
	Empty set (0.00 sec)
	```
2. 创建表
	
	```sql
	mysql> CREATE TABLE pet (name VARCHAR(20), owner VARCHAR(20),
	    -> species VARCHAR(20), sex CHAR(1), birth DATE, death DATE);
	```	
3.	show tables
	
	```sql
	mysql> show tables;
	+---------------------+
	| Tables_in_menagerie |
	+---------------------+
	| pet                 |
	+---------------------+
	1 row in set (0.00 sec)
	```
4.	展示表结构
	
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
	6 rows in set (0.00 sec)

	```
	
## 导入数据到表内
1. 从文件导入	

	```sql
	LOAD DATA LOCAL INFILE '/path/pet.txt' INTO TABLE pet;
	```
	
	如果遇到[The used command is not allowed with this MySQL version](https://stackoverflow.com/questions/18437689/error-1148-the-used-command-is-not-allowed-with-this-mysql-version)
	
	启动数据库的时候加入`--local-infile `

	```	
	mysql -u root -p --local-infile menagerie
	```
	
	修改对应的开关值

	```
	SHOW VARIABLES LIKE 'local_infile';
	
	SET GLOBAL local_infile = 1;
	
	```
	
	
	
	

2. 插入数据 insert

	```sql
	INSERT INTO pet VALUES ('Puffball','Diane','hamster','f','1999-03-30',NULL);
	```

	显示结果
		
	```sql
	mysql> select * from pet;
	+----------+-------+---------+------+------------+-------+
	| name     | owner | species | sex  | birth      | death |
	+----------+-------+---------+------+------------+-------+
	| Puffball | Diane | hamster | f    | 1999-03-30 | NULL  |
	+----------+-------+---------+------+------------+-------+
	1 row in set (0.00 sec)
	```

## 查询数据	

语法
	
```sql
SELECT what_to_select
FROM which_table
WHERE conditions_to_satisfy;
```
	
1. 查询所有数据	

	```sql
	select *from pet;
	mysql> UPDATE pet SET birth = '1989-08-31' WHERE name = 'Bowser';
	mysql> SELECT * FROM pet WHERE name = 'Bowser';
	mysql> SELECT * FROM pet WHERE birth >= '1998-1-1';
	mysql> SELECT * FROM pet WHERE species = 'dog' AND sex = 'f';
	mysql> SELECT * FROM pet WHERE species = 'snake' OR species = 'bird';
	mysql> SELECT * FROM pet WHERE (species = 'cat' AND sex = 'm')  OR (species = 'dog' AND sex = 'f');
	```
2. 查询列
	
	```sql
	mysql> SELECT name, birth FROM pet;
	+----------+------------+
	| name     | birth      |
	+----------+------------+
	| Fluffy   | 1993-02-04 |
	| Claws    | 1994-03-17 |
	| Buffy    | 1989-05-13 |
	| Fang     | 1990-08-27 |
	| Bowser   | 1989-08-31 |
	| Chirpy   | 1998-09-11 |
	| Whistler | 1997-12-09 |
	| Slim     | 1996-04-29 |
	| Puffball | 1999-03-30 |
	+----------+------------+
	```

	查询列并去重
		
	```sql
	mysql> SELECT owner FROM pet;
	+--------+
	| owner  |
	+--------+
	| Harold |
	| Gwen   |
	| Harold |
	| Benny  |
	| Diane  |
	| Gwen   |
	| Gwen   |
	| Benny  |
	| Diane  |
	+--------+
	mysql> SELECT DISTINCT owner FROM pet; 
	+--------+
	| owner  |
	+--------+
	| Benny  |
	| Diane  |
	| Gwen   |
	| Harold |
	+--------+
	```

	and , or 用法
		
	```sql
	mysql> SELECT name, species, birth FROM pet
	-> WHERE species = 'dog' OR species = 'cat';
	+--------+---------+------------+
	| name   | species | birth      |
	+--------+---------+------------+
	| Fluffy | cat     | 1993-02-04 |
	| Claws  | cat     | 1994-03-17 |
	| Buffy  | dog     | 1989-05-13 |
	| Fang   | dog     | 1990-08-27 |
	| Bowser | dog     | 1989-08-31 |
	+--------+---------+------------+
	```
3. 排序,默认是升序排列
	
	```sql
	mysql> SELECT name, birth FROM pet ORDER BY birth;
	+----------+------------+
	| name     | birth      |
	+----------+------------+
	| Buffy    | 1989-05-13 |
	| Bowser   | 1989-08-31 |
	| Fang     | 1990-08-27 |
	| Fluffy   | 1993-02-04 |
	| Claws    | 1994-03-17 |
	| Slim     | 1996-04-29 |
	| Whistler | 1997-12-09 |
	| Chirpy   | 1998-09-11 |
	| Puffball | 1999-03-30 |
	+----------+------------+
	```
		
	降序排列
		
	```sql
	mysql> SELECT name, birth FROM pet ORDER BY birth DESC;
	+----------+------------+
	| name     | birth      |
	+----------+------------+
	| Puffball | 1999-03-30 |
	| Chirpy   | 1998-09-11 |
	| Whistler | 1997-12-09 |
	| Slim     | 1996-04-29 |
	| Claws    | 1994-03-17 |
	| Fluffy   | 1993-02-04 |
	| Fang     | 1990-08-27 |
	| Bowser   | 1989-08-31 |
	| Buffy    | 1989-05-13 |
	+----------+------------+
	```
		
	多列排序,下面先按species升序排列,之后按birth降序排列
	
	```sql
	mysql> SELECT name, species, birth FROM pet
	-> ORDER BY species, birth DESC;
	+----------+---------+------------+
	| name     | species | birth      |
	+----------+---------+------------+
	| Chirpy   | bird    | 1998-09-11 |
	| Whistler | bird    | 1997-12-09 |
	| Claws    | cat     | 1994-03-17 |
	| Fluffy   | cat     | 1993-02-04 |
	| Fang     | dog     | 1990-08-27 |
	| Bowser   | dog     | 1989-08-31 |
	| Buffy    | dog     | 1989-05-13 |
	| Puffball | hamster | 1999-03-30 |
	| Slim     | snake   | 1996-04-29 |
	+----------+---------+------------+
	```
4. 日期计算

	- `CURDATE()` 

	   当前时间
	- `TIMESTAMPDIFF(unit,datetime_expr1,datetime_expr2)`
		
		求两个时间的差值.`uint:`

		*  MONTH
		*  YEAR
		*  MINUTE
		
		```
		mysql> SELECT TIMESTAMPDIFF(MONTH,'2003-02-01','2003-05-01');
	        -> 3
		mysql> SELECT TIMESTAMPDIFF(YEAR,'2002-05-01','2001-01-01');
		        -> -1
		mysql> SELECT TIMESTAMPDIFF(MINUTE,'2003-02-01','2003-05-01 12:05:55');
		        -> 128885
		```
	- `year(date), month(date), day(date)`
	

	查找名字,生日,当前时间,当前时间与生日的年份差值命名为: `age`

	```sql
	mysql> select name, birth, curdate(), timestampdiff(year, birth, curdate()) as age from pet;
	+----------+------------+------------+------+
	| name     | birth      | curdate()  | age  |
	+----------+------------+------------+------+
	| Puffball | 1999-03-30 | 2018-09-04 |   19 |
	| Fluffy   | 1999-02-04 | 2018-09-04 |   19 |
	| Claws    | 1999-03-17 | 2018-09-04 |   19 |
	| Buffy    | 1989-05-13 | 2018-09-04 |   29 |
	| Fang     | 1990-08-27 | 2018-09-04 |   28 |
	| Bowser   | 1979-08-31 | 2018-09-04 |   39 |
	| Chirpy   | 1998-09-11 | 2018-09-04 |   19 |
	| Whistler | 1997-12-09 | 2018-09-04 |   20 |
	| Slim     | 1996-04-29 | 2018-09-04 |   22 |
	+----------+------------+------------+------+
	```
	
	根据年龄升序排序

	```sql
	mysql> select name, birth, curdate(), timestampdiff(year, birth, curdate()) as age from pet order by age;
	+----------+------------+------------+------+
	| name     | birth      | curdate()  | age  |
	+----------+------------+------------+------+
	| Puffball | 1999-03-30 | 2018-09-04 |   19 |
	| Fluffy   | 1999-02-04 | 2018-09-04 |   19 |
	| Claws    | 1999-03-17 | 2018-09-04 |   19 |
	| Chirpy   | 1998-09-11 | 2018-09-04 |   19 |
	| Whistler | 1997-12-09 | 2018-09-04 |   20 |
	| Slim     | 1996-04-29 | 2018-09-04 |   22 |
	| Fang     | 1990-08-27 | 2018-09-04 |   28 |
	| Buffy    | 1989-05-13 | 2018-09-04 |   29 |
	| Bowser   | 1979-08-31 | 2018-09-04 |   39 |
	+----------+------------+------------+------+
	```
	
	过滤死亡日期不为空的情况

	```sql
	mysql> select name, birth, curdate(), timestampdiff(year, birth, curdate()) as age from pet where death is not null  order by age;
	+--------+------------+------------+------+
	| name   | birth      | curdate()  | age  |
	+--------+------------+------------+------+
	| Bowser | 1979-08-31 | 2018-09-04 |   39 |
	+--------+------------+------------+------+
	1 row in set (0.00 sec)
	```
	
	取生日的月份
	
	```sql
	mysql> select name, birth ,month(birth) from pet;
	+----------+------------+--------------+
	| name     | birth      | month(birth) |
	+----------+------------+--------------+
	| Puffball | 1999-03-30 |            3 |
	| Fluffy   | 1999-02-04 |            2 |
	| Claws    | 1999-03-17 |            3 |
	| Buffy    | 1989-05-13 |            5 |
	| Fang     | 1990-08-27 |            8 |
	| Bowser   | 1979-08-31 |            8 |
	| Chirpy   | 1998-09-11 |            9 |
	| Whistler | 1997-12-09 |           12 |
	| Slim     | 1996-04-29 |            4 |
	+----------+------------+--------------+
	```
	过滤生日月份是5月的
	
	
	```sql
	mysql> select name,month(birth) from pet where month(birth) = 5;
	+-------+--------------+
	| name  | month(birth) |
	+-------+--------------+
	| Buffy |            5 |
	+-------+--------------+
	```
	
	下个月生日的
	
	```sql
	mysql> select name, birth from pet where month(birth) = month(date_add(curdate(), interval 1 month));
	```
	
	使用取模函数
	
	```sql
	mysql> select name, birth from pet where month(birth) = mod(month(curdate()), 12) +1;
	```
5. NULL 值
	-  IS NULL
	-  IS NOT NULL
	
	```sql
	mysql> SELECT 1 IS NULL, 1 IS NOT NULL;
	+-----------+---------------+
	| 1 IS NULL | 1 IS NOT NULL |
	+-----------+---------------+
	|         0 |             1 |
	+-----------+---------------+
    ```
    
    不能对NULL值使用 `=, < ,<> `等算数运算,运算后的结果依然是NULL
    
    ```sql
    mysql> SELECT 1 = NULL, 1 <> NULL, 1 < NULL, 1 > NULL;
	+----------+-----------+----------+----------+
	| 1 = NULL | 1 <> NULL | 1 < NULL | 1 > NULL |
	+----------+-----------+----------+----------+
	|     NULL |      NULL |     NULL |     NULL |
	+----------+-----------+----------+----------+
    ```
    
    如果一个字段定义的时候被标记为`not null`, 在插入数据的时候,`0`值或者`''`空字符串是有值的.

   ```sql 
    mysql> SELECT 0 IS NULL, 0 IS NOT NULL, '' IS NULL, '' IS NOT NULL;
	+-----------+---------------+------------+----------------+
	| 0 IS NULL | 0 IS NOT NULL | '' IS NULL | '' IS NOT NULL |
	+-----------+---------------+------------+----------------+
	|         0 |             1 |          0 |              1 |
	+-----------+---------------+------------+----------------+
	```
6. 模式匹配 `like` , `not like`
	
	1. 以`b`开头的
		
		```sql
		mysql> select * from pet where name like 'b%';
		+--------+--------+---------+------+------------+------------+
		| name   | owner  | species | sex  | birth      | death      |
		+--------+--------+---------+------+------------+------------+
		| Buffy  | Harold | dog     | f    | 1989-05-13 | NULL       |
		| Bowser | Bowser | Bowser  | m    | 1979-08-31 | 1995-07-29 |
		+--------+--------+---------+------+------------+------------+
		```
	
	2. 以`fy `结尾的

		```sql
		mysql> select * from pet where name like '%fy';
		+--------+--------+---------+------+------------+-------+
		| name   | owner  | species | sex  | birth      | death |
		+--------+--------+---------+------+------------+-------+
		| Fluffy | Harold | cat     | f    | 1999-02-04 | NULL  |
		| Buffy  | Harold | dog     | f    | 1989-05-13 | NULL  |
		+--------+--------+---------+------+------------+-------+
		```	

	3. 名字里包含一个`w`的

		```sql
		mysql> select * from pet where name like '%w%';
		+----------+--------+---------+------+------------+------------+
		| name     | owner  | species | sex  | birth      | death      |
		+----------+--------+---------+------+------------+------------+
		| Claws    | Gwen   | cat     | m    | 1999-03-17 | NULL       |
		| Bowser   | Bowser | Bowser  | m    | 1979-08-31 | 1995-07-29 |
		| Whistler | Gwen   | bird    | NULL | 1997-12-09 | NULL       |
		+----------+--------+---------+------+------------+------------+
		```
	4. 名字里包含5个字符的,用5个`_`来匹配
		
		```sql
		mysql> SELECT * FROM pet WHERE name LIKE '_____';
		+-------+--------+---------+------+------------+-------+
		| name  | owner  | species | sex  | birth      | death |
		+-------+--------+---------+------+------------+-------+
		| Claws | Gwen   | cat     | m    | 1999-03-17 | NULL  |
		| Buffy | Harold | dog     | f    | 1989-05-13 | NULL  |
		+-------+--------+---------+------+------------+-------+
		```
	5. 使用正则匹配 `REGEXP_LIKE()`
		- `.`匹配任何单个字符
		- `[...]` 匹配括号里的字符,
			- `[abc]`匹配`a,b,c`
			- `[a-z]`,`[0-9]`匹配范围内的字符
		- `*`匹配0个或多个之前的字符.
			- `x.*` 匹配任意个`x`
			- `.*` 匹配任意个字符
		- `^`匹配字符, `$`匹配结束
		- `{n}`前面字符重复n次
		
		
		正则匹配,以`b`开头

		```sql
		mysql> SELECT * FROM pet WHERE REGEXP_LIKE(name, '^b');
		+--------+--------+---------+------+------------+------------+
		| name   | owner  | species | sex  | birth      | death      |
		+--------+--------+---------+------+------------+------------+
		| Buffy  | Harold | dog     | f    | 1989-05-13 | NULL       |
		| Bowser | Bowser | Bowser  | m    | 1979-08-31 | 1995-07-29 |
		+--------+--------+---------+------+------------+------------+
		```
		
		大小写敏感
		
		- 使用 `BINARY` 关键字
		- 指定  c match-control character
		- 使用一个 case-sensitive collation,
		
		```sql
		SELECT * FROM pet WHERE REGEXP_LIKE(name, '^b' COLLATE utf8mb4_0900_as_cs);
		SELECT * FROM pet WHERE REGEXP_LIKE(name, BINARY '^b');
		SELECT * FROM pet WHERE REGEXP_LIKE(name, '^b', 'c');
		```
		
		以`fy`结尾

		```sql
		mysql>  SELECT * FROM pet WHERE REGEXP_LIKE(name, 'fy$');
		+--------+--------+---------+------+------------+-------+
		| name   | owner  | species | sex  | birth      | death |
		+--------+--------+---------+------+------------+-------+
		| Fluffy | Harold | cat     | f    | 1999-02-04 | NULL  |
		| Buffy  | Harold | dog     | f    | 1989-05-13 | NULL  |
		+--------+--------+---------+------+------------+-------+
		```
		
		包含`w`的

		```sql
		mysql> SELECT * FROM pet WHERE REGEXP_LIKE(name, 'w');
		+----------+--------+---------+------+------------+------------+
		| name     | owner  | species | sex  | birth      | death      |
		+----------+--------+---------+------+------------+------------+
		| Claws    | Gwen   | cat     | m    | 1999-03-17 | NULL       |
		| Bowser   | Bowser | Bowser  | m    | 1979-08-31 | 1995-07-29 |
		| Whistler | Gwen   | bird    | NULL | 1997-12-09 | NULL       |
		+----------+--------+---------+------+------------+------------+
		```
		
		名字长度为5个字符的

		```sql
		mysql> SELECT * FROM pet WHERE REGEXP_LIKE(name, '^.....$');
		+-------+--------+---------+------+------------+-------+
		| name  | owner  | species | sex  | birth      | death |
		+-------+--------+---------+------+------------+-------+
		| Claws | Gwen   | cat     | m    | 1999-03-17 | NULL  |
		| Buffy | Harold | dog     | f    | 1989-05-13 | NULL  |
		+-------+--------+---------+------+------------+-------+
		```		
		使用重复运算符`{n}`

		```sql
		mysql> SELECT * FROM pet WHERE REGEXP_LIKE(name, '^.{5}$');
		+-------+--------+---------+------+------------+-------+
		| name  | owner  | species | sex  | birth      | death |
		+-------+--------+---------+------+------------+-------+
		| Claws | Gwen   | cat     | m    | 1999-03-17 | NULL  |
		| Buffy | Harold | dog     | f    | 1989-05-13 | NULL  |
		+-------+--------+---------+------+------------+-------+
		```		
7. 计算记录的数量, `count()`
	
	查看所有的记录条数
	
	```sql
	mysql> select count(*) from pet;
	+----------+
	| count(*) |
	+----------+
	|        9 |
	+----------+
	```
	
	查询宠物主人有多少个宠物
	
	```sql
	mysql> select owner, count(*) from pet group by owner;
	+--------+----------+
	| owner  | count(*) |
	+--------+----------+
	| Diane  |        1 |
	| Harold |        2 |
	| Gwen   |        3 |
	| Benny  |        2 |
	| Bowser |        1 |
	+--------+----------+
	```
	
	每个种类有多少个
	
	```sql
	mysql> SELECT species, COUNT(*) FROM pet GROUP BY species;
	+---------+----------+
	| species | COUNT(*) |
	+---------+----------+
	| hamster |        1 |
	| cat     |        2 |
	| dog     |        2 |
	| Bowser  |        1 |
	| bird    |        2 |
	| snake   |        1 |
	+---------+----------+
	```
	
	每个性别多少只
	
	```sql
	mysql> SELECT sex, COUNT(*) FROM pet GROUP BY sex;
	+------+----------+
	| sex  | COUNT(*) |
	+------+----------+
	| f    |        4 |
	| m    |        4 |
	| NULL |        1 |
	+------+----------+
	```
	
	根据种类和性别分组,计算每个分组多少只
	
	```sql
	mysql> SELECT species, sex, COUNT(*) FROM pet GROUP BY species, sex;
	+---------+------+----------+
	| species | sex  | COUNT(*) |
	+---------+------+----------+
	| hamster | f    |        1 |
	| cat     | f    |        1 |
	| cat     | m    |        1 |
	| dog     | f    |        1 |
	| dog     | m    |        1 |
	| Bowser  | m    |        1 |
	| bird    | f    |        1 |
	| bird    | NULL |        1 |
	| snake   | m    |        1 |
	+---------+------+----------+
	```
	
	只计算猫和狗,根据种类和性别分组,计算每个分组多少只
	
	```sql
	mysql> select species, sex, count(*)  from pet where species = 'dog' or species = 'cat' group by species, sex;
	+---------+------+----------+
	| species | sex  | count(*) |
	+---------+------+----------+
	| cat     | f    |        1 |
	| cat     | m    |        1 |
	| dog     | f    |        1 |
	| dog     | m    |        1 |
	+---------+------+----------+
	```
	
	只计算sex 不为空的情况,根据种类和性别分组,计算每个分组多少只
	
	```sql
	mysql> select species, sex, count(*)  from pet where sex is not null  group by species, sex;
	+---------+------+----------+
	| species | sex  | count(*) |
	+---------+------+----------+
	| hamster | f    |        1 |
	| cat     | f    |        1 |
	| cat     | m    |        1 |
	| dog     | f    |        1 |
	| dog     | m    |        1 |
	| Bowser  | m    |        1 |
	| bird    | f    |        1 |
	| snake   | m    |        1 |
	+---------+------+----------+
	```
8. 多表查询
   1. 创建event表

		```sql
		mysql> CREATE TABLE event (name VARCHAR(20), date DATE,
		    -> type VARCHAR(15), remark VARCHAR(255));
		```

   2. load data from file
		
		name|	date|	type|	remark
		----|------|------|------
		Fluffy	|1995-05-15	|litter	|4 kittens, 3 female, 1 male
		Buffy	|1993-06-23	|litter	|5 puppies, 2 female, 3 male
		Buffy	|1994-06-19	|litter	|3 puppies, 3 female
		Chirpy	|1999-03-21	|vet	|needed beak straightened
		Slim	|1997-08-03	|vet	|broken rib
		Bowser	|1991-10-12	|kennel |	
		Fang	|1991-10-12	|kennel	|
		Fang	|1998-08-28	|birthday	|Gave him a new chew toy
		Claws	|1998-03-17	|birthday	|Gave him a new flea collar
		Whistler	|1998-12-09	birthday	|First birthday

		```sql
		mysql> LOAD DATA LOCAL INFILE 'event.txt' INTO TABLE event;
		mysql> show tables;
		+---------------------+
		| Tables_in_menagerie |
		+---------------------+
		| event               |
		| pet                 |
		+---------------------+
		```

   3. 连接查询
   
		```  sql
		mysql> SELECT pet.name,
		    -> TIMESTAMPDIFF(YEAR,birth,date) AS age,
		    -> remark
		    -> FROM pet INNER JOIN event
		    ->   ON pet.name = event.name
		    -> WHERE event.type = 'litter';
		+--------+------+-----------------------------+
		| name   | age  | remark                      |
		+--------+------+-----------------------------+
		| Fluffy |    2 | 4 kittens, 3 female, 1 male |
		| Buffy  |    4 | 5 puppies, 2 female, 3 male |
		| Buffy  |    5 | 3 puppies, 3 female         |
		+--------+------+-----------------------------+
		```
		
		相同的表也可以连接查询
		
		```sql
		mysql> SELECT p1.name, p1.sex, p2.name, p2.sex, p1.species
		    -> FROM pet AS p1 INNER JOIN pet AS p2
		    ->   ON p1.species = p2.species AND p1.sex = 'f' AND p2.sex = 'm';
		+--------+------+--------+------+---------+
		| name   | sex  | name   | sex  | species |
		+--------+------+--------+------+---------+
		| Fluffy | f    | Claws  | m    | cat     |
		| Buffy  | f    | Fang   | m    | dog     |
		| Buffy  | f    | Bowser | m    | dog     |
		+--------+------+--------+------+---------+
		```	

	
	
	

