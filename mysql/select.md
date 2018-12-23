1. ==SQL IN==
	
	IN 操作符允许我们在 WHERE 子句中规定多个值。

	```sql
	SELECT column_name(s)
	FROM table_name
	WHERE column_name IN (value1,value2,...)
	```
	example
	
	```sql
	SELECT * FROM Persons
	WHERE LastName IN ('Adams','Carter')
	```
2. ==LIMIT==
	
	LIMIT 子句用于规定要返回的记录的数目.
	
	```sql
	SELECT column_name(s)
	FROM table_name
	LIMIT number
	```
	example
	
	```sql
	SELECT *
	FROM Persons
	LIMIT 5
	```
3. ==BETWEEN==

	操作符 BETWEEN ... AND 会选取介于两个值之间的数据范围。这些值可以是数值、文本或者日期。
	
	```sql
	SELECT column_name(s)
	FROM table_name
	WHERE column_name
	BETWEEN value1 AND value2
	```
	example
	
	```sql
	SELECT * FROM Persons
	WHERE LastName
	BETWEEN 'Adams' AND 'Carter'
	```
	
	重要事项：不同的数据库对 BETWEEN...AND 操作符的处理方式是有差异的。某些数据库会列出介于 "Adams" 和 "Carter" 之间的人，但不包括 "Adams" 和 "Carter" ；某些数据库会列出介于 "Adams" 和 "Carter" 之间并包括 "Adams" 和 "Carter" 的人；而另一些数据库会列出介于 "Adams" 和 "Carter" 之间的人，包括 "Adams" ，但不包括 "Carter" 。
	
	==NOT BETWEEN==
	
	如需显示范围之外的，请使用 NOT 操作符：
	
	```sql
	SELECT * FROM Persons
	WHERE LastName
	NOT BETWEEN 'Adams' AND 'Carter'
	```
		
	
4. ==SQL Alias==
	
	表的 SQL Alias 语法
	
	```sql
	SELECT column_name(s)
	FROM table_name
	AS alias_name
	```          
	列的 SQL Alias 语法	

	```sql
	SELECT column_name AS alias_name
	FROM table_name
	```           
5. ==JOIN==
	
	- inner join
	- left join
	- right join
	- full join
6. ==UNION==
	
	SQL UNION 语法
	
	```sql
	SELECT column_name(s) FROM table_name1
	UNION
	SELECT column_name(s) FROM table_name2
	```
	注释：默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。

	SQL UNION ALL 语法
	
	```sql
	SELECT column_name(s) FROM table_name1
	UNION ALL
	SELECT column_name(s) FROM table_name2
	```
	另外，UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。


7. ==SELECT INTO== 和 ==IN==

	SELECT INTO 语句从一个表中选取数据，然后把数据插入另一个表中。</br>
	SELECT INTO 语句常用于创建表的备份复件或者用于对记录进行存档。
	
	把所有的列插入新表：
	
	```sql
	SELECT *
	INTO new_table_name [IN externaldatabase] 
	FROM old_tablename
	```
	只把希望的列插入新表：

	```sql
	SELECT column_name(s)
	INTO new_table_name [IN externaldatabase] 
	FROM old_tablename
	```
	
	**SQL SELECT INTO 实例 - 制作备份复件**

	下面的例子会制作 "Persons" 表的备份复件：

	```
	SELECT *
	INTO Persons_backup
	FROM Persons
	```
	
	IN 子句可用于向另一个数据库中拷贝表：
	
	```sql
	SELECT *
	INTO Persons IN 'Backup.mdb'
	FROM Persons
	```
	如果我们希望拷贝某些域，可以在 SELECT 语句后列出这些域：
	
	```sql
	SELECT LastName,FirstName
	INTO Persons_backup
	FROM Persons
	```
	
	**SQL SELECT INTO 实例 - 带有 WHERE 子句**

	我们也可以添加 WHERE 子句。</br>
	下面的例子通过从 "Persons" 表中提取居住在 "Beijing" 的人的信息，创建了一个带有两个列的名为 "Persons_backup" 的表：

	```sql
	SELECT LastName,Firstname
	INTO Persons_backup
	FROM Persons
	WHERE City='Beijing'
	SQL SELECT INTO 实例 - 被连接的表
	```
	从一个以上的表中选取数据也是可以做到的。
	
	下面的例子会创建一个名为 "Persons_Order_Backup" 的新表，其中包含了从 Persons 和 Orders 两个表中取得的信息：
	
	```sql
	SELECT Persons.LastName,Orders.OrderNo
	INTO Persons_Order_Backup
	FROM Persons
	INNER JOIN Orders
	ON Persons.Id_P=Orders.Id_P
	```
	