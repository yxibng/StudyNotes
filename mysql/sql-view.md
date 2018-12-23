# SQL VIEW（视图）
 什么是视图？
> 在 SQL 中，视图是基于 SQL 语句的结果集的可视化的表。</br>
视图包含行和列，就像一个真实的表。视图中的字段就是来自一个或多个数据库中的真实的表中的字段。我们可以向视图添加 SQL 函数、WHERE 以及 JOIN 语句，我们也可以提交数据，就像这些来自于某个单一的表。

注释：数据库的设计和结构不会受到视图中的函数、where 或 join 语句的影响。

允许用户执行以下操作：

- 以用户或者某些类型的用户感觉自然或者直观的方式来组织数据；
- 限制对数据的访问，从而使得用户仅能够看到或者修改（某些情况下）他们需要的数据；
- 从多个表中汇总数据，以产生报表。

## SQL CREATE VIEW 语法
数据库视图由 CREATE VIEW 语句创建。视图可以创建自单个表、多个表或者其他视图。</br>
要创建视图的话，用户必须有适当的系统权限。具体需要何种权限随数据库系统实现的不同而不同。

```sql
CREATE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition
```
注释：视图总是显示最近的数据。每当用户查询视图时，数据库引擎通过使用 SQL 语句来重建


### 示例
> table customers

```
+----+----------+-----+-----------+----------+
| ID | NAME     | AGE | ADDRESS   | SALARY   |
+----+----------+-----+-----------+----------+
|  1 | Ramesh   |  32 | Ahmedabad |  2000.00 |
|  2 | Khilan   |  25 | Delhi     |  1500.00 |
|  3 | kaushik  |  23 | Kota      |  2000.00 |
|  4 | Chaitali |  25 | Mumbai    |  6500.00 |
|  5 | Hardik   |  27 | Bhopal    |  8500.00 |
|  6 | Komal    |  22 | MP        |  4500.00 |
|  7 | Muffy    |  24 | Indore    | 10000.00 |
+----+----------+-----+-----------+----------+

```
下面是由 CUSTOMERS 表创建视图的例子。该视图包含来自 CUSTOMERS 表的顾客的名字（name）和年龄（age）：

```sql
SQL > CREATE VIEW CUSTOMERS_VIEW AS
SELECT name, age
FROM  CUSTOMERS;
```
现在，你就可以像查询普通的数据表一样查询 CUSTOMERS_VIEW 了：

```sql
SQL > SELECT * FROM CUSTOMERS_VIEW;
```
上述语句将会产生如下运行结果：

```
+----------+-----+
| name     | age |
+----------+-----+
| Ramesh   |  32 |
| Khilan   |  25 |
| kaushik  |  23 |
| Chaitali |  25 |
| Hardik   |  27 |
| Komal    |  22 |
| Muffy    |  24 |
+----------+-----+
```
### WITH CHECK OPTION

WITH CHECK OPTION 是 CREATE VIEW 语句的一个可选项。WITH CHECK OPTION 用于保证所有的 UPDATE 和 INSERT 语句都满足视图定义中的条件。

如果不能满足这些条件，UPDATE 或 INSERT 就会返回错误。

下面的例子创建的也是 CUSTOMERS_VIEW 视图，不过这次 WITH CHECK OPTION 是打开的：

```sql
CREATE VIEW CUSTOMERS_VIEW AS
SELECT name, age
FROM  CUSTOMERS
WHERE age IS NOT NULL
WITH CHECK OPTION;
```
这里 WITH CHECK OPTION 使得视图拒绝任何 AGE 字段为 NULL 的条目，因为视图的定义中，AGE 字段不能为空。


## SQL 更新视图
您可以使用下面的语法来更新视图：

```sql
SQL CREATE OR REPLACE VIEW Syntax
CREATE OR REPLACE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition
```

视图可以在特定的情况下更新：

- SELECT 子句不能包含 DISTINCT 关键字
- SELECT 子句不能包含任何汇总函数（summary functions）
- SELECT 子句不能包含任何集合函数（set functions）
- SELECT 子句不能包含任何集合运算符（set operators）
- SELECT 子句不能包含 ORDER BY 子句
- FROM 子句中不能有多个数据表
- WHERE 子句不能包含子查询（subquery）
- 查询语句中不能有 GROUP BY 或者 HAVING
- 计算得出的列不能更新
- 视图必须包含原始数据表中所有的 NOT NULL 列，从而使 INSERT 查询生效。

如果视图满足以上所有的条件，该视图就可以被更新。下面的例子中，Ramesh 的年龄被更新了：

```sql
SQL > UPDATE CUSTOMERS_VIEW
      SET AGE = 35
      WHERE name='Ramesh';
```
最终更新的还是原始数据表，只是其结果反应在了视图上。现在查询原始数据表，SELECT 语句将会产生以下结果：

```
+----+----------+-----+-----------+----------+
| ID | NAME     | AGE | ADDRESS   | SALARY   |
+----+----------+-----+-----------+----------+
|  1 | Ramesh   |  35 | Ahmedabad |  2000.00 |
|  2 | Khilan   |  25 | Delhi     |  1500.00 |
|  3 | kaushik  |  23 | Kota      |  2000.00 |
|  4 | Chaitali |  25 | Mumbai    |  6500.00 |
|  5 | Hardik   |  27 | Bhopal    |  8500.00 |
|  6 | Komal    |  22 | MP        |  4500.00 |
|  7 | Muffy    |  24 | Indore    | 10000.00 |
+----+----------+-----+-----------+----------+
```

### 向视图中插入新行
可以向视图中插入新行，其规则同（使用 UPDATE 命令）更新视图所遵循的规则相同。

这里我们不能向 CUSTOMERS_VIEW 视图中添加新行，因为该视图没有包含原始数据表中所有 NOT NULL 的列。否则的话，你就可以像在数据表中插入新行一样，向视图中插入新行。

### 删除视图中的行：
视图中的数据行可以被删除。删除数据行与更新视图和向视图中插入新行遵循相同的规则。

下面的例子将删除 CUSTOMERS_VIEW 视图中 AGE=22 的数据行：

```sql
SQL > DELETE FROM CUSTOMERS_VIEW
      WHERE age = 22;
```
该语句最终会将原始数据表中对应的数据行删除，只不过其结果反应在了视图上。现在查询原始数据表，SELECT 语句将会产生以下结果：

```
+----+----------+-----+-----------+----------+
| ID | NAME     | AGE | ADDRESS   | SALARY   |
+----+----------+-----+-----------+----------+
|  1 | Ramesh   |  35 | Ahmedabad |  2000.00 |
|  2 | Khilan   |  25 | Delhi     |  1500.00 |
|  3 | kaushik  |  23 | Kota      |  2000.00 |
|  4 | Chaitali |  25 | Mumbai    |  6500.00 |
|  5 | Hardik   |  27 | Bhopal    |  8500.00 |
|  7 | Muffy    |  24 | Indore    | 10000.00 |
+----+----------+-----+-----------+----------+
```

## SQL 撤销视图
您可以通过 DROP VIEW 命令来删除视图。

```sql
SQL DROP VIEW Syntax
DROP VIEW view_name
```