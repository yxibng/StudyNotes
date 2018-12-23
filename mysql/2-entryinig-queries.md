1. 查询版本信息,当前日期

	```
	mysql> select version(),current_date;
	+-----------+--------------+
	| version() | current_date |
	+-----------+--------------+
	| 8.0.12    | 2018-09-03   |
	+-----------+--------------+
	1 row in set (0.00 sec)
	```
	
	关键字不区分大小写

	```
	mysql> SELECT VERSION(), CURRENT_DATE;
	mysql> select version(), current_date;
	mysql> SeLeCt vErSiOn(), current_DATE;
	```
	
2. 可以当计算器用

	```
	mysql> SELECT SIN(PI()/4), (4+1)*5;
	+--------------------+---------+
	| SIN(PI()/4)        | (4+1)*5 |
	+--------------------+---------+
	| 0.7071067811865475 |      25 |
	+--------------------+---------+
	1 row in set (0.01 sec)
	```
3. 一行写多个语句,用`;`分割

	```
	mysql> SELECT VERSION(); SELECT NOW();
	+-----------+
	| VERSION() |
	+-----------+
	| 8.0.12    |
	+-----------+
	1 row in set (0.00 sec)
	
	+---------------------+
	| NOW()               |
	+---------------------+
	| 2018-09-03 15:52:29 |
	+---------------------+
	1 row in set (0.00 sec)
	```
4. 一条语句分多行去写,遇到`;`结束此条语句

	```
	mysql> select
	    -> user()
	    -> ,
	    -> current_date;
	+----------------+--------------+
	| user()         | current_date |
	+----------------+--------------+
	| root@localhost | 2018-09-03   |
	+----------------+--------------+
	1 row in set (0.00 sec)
	```
5. 取消一个查询语句,输入`\c`
	
	```
	mysql> select
	    -> user()
	    ->
	    -> \c
	mysql>
	```
	
prompt | Meaning
-------|-------
mysql> | 等待下一个查询开始
->     | 等待多行查询的下一行
' >    | 等待以`'`结束的下一行
">     | 等待以`"`结束的下一行
\`>    | 等待以 \` 结束的下一行
/*>    | 等待以 `/* `结束的下一行


```
mysql> SELECT USER()
    ->
```	


```
mysql> SELECT USER()
    -> ;
+---------------+
| USER()        |
+---------------+
| jon@localhost |
+---------------+
```

```
mysql> SELECT * FROM my_table WHERE name = 'Smith AND age < 30;
    '>
```	

```
mysql> SELECT * FROM my_table WHERE name = 'Smith AND age < 30;
    '> '\c
mysql>
```


	