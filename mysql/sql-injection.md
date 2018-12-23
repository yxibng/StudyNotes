# SQL Injection

SQL injection is a code injection technique that might destroy your database.

SQL injection is one of the most common web hacking techniques.

SQL injection is the placement of malicious code in SQL statements, via web page input.



## SQL in Web Pages

所谓SQL注入，就是通过把SQL命令插入到Web表单递交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令。
我们永远不要信任用户的输入，我们必须认定用户输入的数据都是不安全的，我们都需要对用户输入的数据进行过滤处理。

example

```
txtUserId = getRequestString("UserId");
txtSQL = "SELECT * FROM Users WHERE UserId = " + txtUserId;

```
### SQL Injection Based on 1=1 is Always True

用户输入了 `UserId: 105 OR 1=1`</br>
之后,sql语句变成了:

```sql
SELECT * FROM Users WHERE UserId = 105 OR 1=1;
```
这条sql语句将返回用户表中的所有数据,因为 `OR 1=1` 永远是true. 上面的语句看起来非常危险,想象一下用户表包含了名字和密码.

```sql
SELECT UserId, Name, Password FROM Users WHERE UserId = 105 or 1=1;
```
通过这样的语句,就可以获取数据库中所有用户的用户名和密码,只需要输入`105 OR 1=1`.

### SQL Injection Based on ""="" is Always True

两个输入框:

- Username: John Doe
- Password : myPass

example

```sql
uName = getRequestString("username");
uPass = getRequestString("userpassword");

sql = 'SELECT * FROM Users WHERE Name ="' + uName + '" AND Pass ="' + uPass + '"'
```
result 

```sql
SELECT * FROM Users WHERE Name ="John Doe" AND Pass ="myPass"
```

此时可以通过注入`OR ""=""`的方式获取用户名和密码

- UserNmae: `"or"="`
- Password: `"or""=`

拼凑的sql语句如下:

```sql
SELECT * FROM Users WHERE Name ="" or ""="" AND Pass ="" or ""=""
```
上面的语句是合法的,并且将返回`Users`表中所有记录,因为`OR ""=""`一直是true.

### SQL Injection Based on Batched SQL Statements 

批处理:执行一组用`;`分割的多个sql语句.

example_1

```sql
SELECT * FROM Users; DROP TABLE Suppliers
```
example_2

```sql
txtUserId = getRequestString("UserId");
txtSQL = "SELECT * FROM Users WHERE UserId = " + txtUserId;
```
输入 `User id: 105; DROP TABLE Suppliers`,sql语句如下

```sql
SELECT * FROM Users WHERE UserId = 105; DROP TABLE Suppliers;
```

变成example_1的语句,将删除Suppliers表,产生很严重的后果.


## Use SQL Parameters for Protection

To protect a web site from SQL injection, you can use SQL parameters.

SQL parameters are values that are added to an SQL query at execution time, in a controlled manner.

```
txtNam = getRequestString("CustomerName");
txtAdd = getRequestString("Address");
txtCit = getRequestString("City");
txtSQL = "INSERT INTO Customers (CustomerName,Address,City) Values(@0,@1,@2)";
db.Execute(txtSQL,txtNam,txtAdd,txtCit);
```

防止SQL注入，我们需要注意以下几个要点：

1. 永远不要信任用户的输入。对用户的输入进行校验，可以通过正则表达式，或限制长度；对单引号和 双"-"进行转换等。
2. 永远不要使用动态拼装sql，可以使用参数化的sql或者直接使用存储过程进行数据查询存取。
3. 永远不要使用管理员权限的数据库连接，为每个应用使用单独的权限有限的数据库连接。
4. 不要把机密信息直接存放，加密或者hash掉密码和敏感的信息。
5. 应用的异常信息应该给出尽可能少的提示，最好使用自定义的错误信息对原始错误信息进行包装
6. sql注入的检测方法一般采取辅助软件或网站平台来检测，软件一般采用sql注入检测工具jsky，网站平台就有亿思网站安全平台检测工具。MDCSOFT SCAN等。采用MDCSOFT-IPS可以有效的防御SQL注入，XSS攻击等。


### 防止SQL注入
在脚本语言，如Perl和PHP你可以对用户输入的数据进行转义从而来防止SQL注入。

PHP的MySQL扩展提供了mysqli_real_escape_string()函数来转义特殊的输入字符。

```php
if (get_magic_quotes_gpc()) 
{
  $name = stripslashes($name);
}
$name = mysqli_real_escape_string($conn, $name);
 mysqli_query($conn, "SELECT * FROM users WHERE name='{$name}'");
```

### Like语句中的注入
like查询时，如果用户输入的值有"_"和"%"，则会出现这种情况：用户本来只是想查询"abcd_"，查询结果中却有"abcd_"、"abcde"、"abcdf"等等；用户要查询"30%"（注：百分之三十）时也会出现问题。

在PHP脚本中我们可以使用addcslashes()函数来处理以上情况，如下实例：

```php
$sub = addcslashes(mysqli_real_escape_string($conn, "%something_"), "%_");
// $sub == \%something\_
 mysqli_query($conn, "SELECT * FROM messages WHERE subject LIKE '{$sub}%'");
```

