#JOIN Syntax 连接语法

JOIN 按照功能大概分为4类: `内连接,左(外)连接,右(外)连接,全(外)连接`

- **(INNER) JOIN**: Returns records that have matching values in both tables
- **LEFT (OUTER) JOIN**: Return all records from the left table, and the matched records from the right table
- **RIGHT (OUTER) JOIN**: Return all records from the right table, and the matched records from the left table
- **FULL (OUTER) JOIN**: Return all records when there is a match in either left or right table

<figure class="half">
    <img src="https://www.w3schools.com/sql/img_innerjoin.gif">
    <img src="https://www.w3schools.com/sql/img_leftjoin.gif">
</figure>

<figure class="half">
    <img src="https://www.w3schools.com/sql/img_rightjoin.gif">
    <img src="https://www.w3schools.com/sql/img_fulljoin.gif">
</figure>


## Inner join

<img src="https://www.w3schools.com/sql/img_innerjoin.gif">

### syntax
```sql
SELECT column_name(s)
FROM table1
INNER JOIN table2 ON table1.column_name = table2.column_name;
```
### [example](https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_join_inner)

```sql
SELECT Orders.OrderID, Customers.CustomerName
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
```
### [join three tables](https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_join_inner2)

```sql
SELECT Orders.OrderID, Customers.CustomerName, Shippers.ShipperName
FROM ((Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID)
INNER JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID);
```




## Left join

<img src="https://www.w3schools.com/sql/img_leftjoin.gif">
### syntax

```sql
SELECT column_name(s)
FROM table1
LEFT JOIN table2 ON table1.column_name = table2.column_name;
```

### [example](https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_join_left)

```sql
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID
ORDER BY Customers.CustomerName;
```

## Right join
<img src="https://www.w3schools.com/sql/img_rightjoin.gif">
### syntax

```sql
SELECT column_name(s)
FROM table1
RIGHT JOIN table2 ON table1.column_name = table2.column_name;
```
### [example](https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_join_right&ss=-1)

```sql
SELECT Orders.OrderID, Employees.LastName, Employees.FirstName
FROM Orders
RIGHT JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID
ORDER BY Orders.OrderID;
```

## full outer join
<img src="https://www.w3schools.com/sql/img_fulljoin.gif">

### syntax

```sql
SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2 ON table1.column_name = table2.column_name;
```

### example

```sql
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
FULL OUTER JOIN Orders ON Customers.CustomerID=Orders.CustomerID
ORDER BY Customers.CustomerName;
```

## self join 
A self JOIN is a regular join, but the table is joined with itself.

### syntax

```sql
SELECT column_name(s)
FROM table1 T1, table1 T2
WHERE condition;
```

### [example](https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_join_self)

```sql
SELECT A.CustomerName AS CustomerName1, B.CustomerName AS CustomerName2, A.City
FROM Customers A, Customers B
WHERE A.CustomerID <> B.CustomerID
AND A.City = B.City 
ORDER BY A.City;
```




当编写join时,需要了解的知识点

1. 数据表别名, 可以通过下面的语法指定

	- `tbl_name AS alias_name`
	- `tbl_name alias_name`
	
	```sql
	SELECT t1.name, t2.salary
	  FROM employee AS t1 INNER JOIN info AS t2 ON t1.name = t2.name;
	
	SELECT t1.name, t2.salary
	  FROM employee t1 INNER JOIN info t2 ON t1.name = t2.name;
	```
	
	
2. 子查询表,从table衍生,或者从一个查询中衍生

	```sql
	SELECT * FROM (SELECT 1, 2, 3) AS t1;
	```
3. 用`on`的地方都可以用`where`替代.通常情况下,`on`用来指定两个表如何连接,`where`用来限制查询的结果.

4. 使用`on`或`using`在进行左连接的时候,如果右边的表没有任何匹配记录,一条值全`NULL`的记录会出现在连接结果右表中.这个可以用来检测当前表与另外一张表没有对应的部分.
	
5. `using(column_list)` 指定了一个列表,列表中的每一项都要同时存在于两张表里. 
	
	表a和表b,同时存在了c1,c2,c3,下面的连接对比两个表中对应的列.
	
	```sql
	a LEFT JOIN b USING (c1, c2, c3)
	```



