# The ENUM Type
> An ENUM is a string object with a value chosen from a list of permitted values that are enumerated explicitly in the column specification at table creation time




advantages:

- Compact data storage in situations where a column has a limited set of possible values. The strings you specify as input values are automatically encoded as number
- Readable queries and output. The numbers are translated back to the corresponding strings in query results.
- 存储更紧凑,指定的输入字符串会自动被编码成数字
- 可读的输入和输出. 查询的时候,对应的编码会被反解成对应的字符串.


### Creating and Using ENUM Columns

值必须是`''`括起来的字符串

```sql
CREATE TABLE shirts (
    name VARCHAR(40),
    size ENUM('x-small', 'small', 'medium', 'large', 'x-large')
);
INSERT INTO shirts (name, size) VALUES ('dress shirt','large'), ('t-shirt','medium'),
  ('polo shirt','small');
SELECT name, size FROM shirts WHERE size = 'medium';
+---------+--------+
| name    | size   |
+---------+--------+
| t-shirt | medium |
+---------+--------+
UPDATE shirts SET size = 'small' WHERE size = 'large';
COMMIT;
```
插入100万条`size='medium'`记录,需要100万字节的存储,相反如果指定size类型为varchar则需要600万字节的存储.

### Index Values for Enumeration Literals
每个枚举值对应一个下标,可以通过下标来进行查询.

- 下标从 1 开始
- 空字符串枚举值,代表错误枚举,下标为0. 通过下面的查询可以找出设置了错误枚举值的记录. 

	```sql
	mysql> SELECT * FROM tbl_name WHERE enum_col=0;
	```
- NULL 的 下标为 NULL

## Empty or NULL Enumeration Values
在下面的情况下,枚举的值可以是`''`或NULL

- 如果插入错误的数据到枚举列,一个空字符串会被插入到这列,这个字符串会区别于正常的空字符串,这个空字符串的索引下标为0.在指定了严格模式的情况下,插入非法枚举值,是不被允许的,会报错.
- 如果某个枚举列允许为NULL,此时NULL就是合法的值,可以插入NULL到这一列,且默认值是NULL.如果指定NOT NULL, 默认值就是第一个枚举中的第一个元素.

## Enumeration Sorting

根据枚举列排序的时候,依据的是枚举值的下标. 

-  在枚举ENUM('b','a')中'b' 就排在 'a'的前面.
-  空字符串排序在非空字符串的前面
-  NULL值排在最前面

## Enumeration Limitations

- 不能是一个表达式,即使表达式的结果是字符串集合
	
	```sql
	CREATE TABLE sizes (
	    size ENUM('small', CONCAT('med','ium'), 'large')
	);
	```
- 不能是用户定义的变量

	```sql
	SET @mysize = 'medium';
	
	CREATE TABLE sizes (
	    size ENUM('small', @mysize, 'large')
	);
	```
	
	
### 强烈不推荐用户使用数字作为枚举值

- 不节省存储空间
- 容易和字符串数字搞混.
- 反正就是不好
	
	
	