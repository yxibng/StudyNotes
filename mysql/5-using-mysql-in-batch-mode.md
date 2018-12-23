#mysql 批处理

1. 基本语法

	可以把多个sql语句放到一个文件当中,然后告诉mysql从文件中读取sql语句并执行
	
	```shell
	shell> mysql < batch-file
	```
	或者
	
	```
	shell> mysql -h host -u user -p < batch-file
	Enter password: ********```
	```

2. 示例:
	> batch.txt
	
	```sql
	select * from pet,
	select * from event;
	```
	
	shell
	
	```
 	shell> mysql -u root -p menagerie  < ~/Desktop/NoteBook/mysql/batch.txt
	```
	
	输出结果
	
	```
	name	owner	species	sex	birth	death
	Fluffy	Harold	cat	f	1993-02-04	NULL
	Claws	Gwen	cat	m	1994-03-17	NULL
	Buffy	Harold	dog	f	1989-05-13	NULL
	Fang	Benny	dog	m	1990-08-27	NULL
	Bowser	Diane	dog	m	1979-08-31	1995-07-29
	Chirpy	Gwen	bird	f	1998-09-11	NULL
	Whistler	Gwen	bird	NULL	1997-12-09	NULL
	Slim	Benny	snake	m	1996-04-29	NULL
	name	date	type	remark
	Fluffy	1995-05-15	litter	4 kittens, 3 female, 1 male
	Buffy	1993-06-23	litter	5 puppies, 2 female, 3 male
	Buffy	1994-06-19	litter	3 puppies, 3 female
	Chirpy	1999-03-21	vet	needed beak straightened
	Slim	1997-08-03	vet	broken rib
	Bowser	1991-10-12	kennel
	Fang	1991-10-12	kennel
	Fang	1998-08-28	birthday	Gave him a new chew toy
	Claws	1998-03-17	birthday	Gave him a new flea collar
	Whistler	1998-12-09	birthday	First birthday
	```
3. 搭配其它用法

	- 输出过多,按页浏览

		```sql
		shell> mysql < batch-file | more
		```
	- 把输出写入一个文件

		```sql
		shell> mysql < batch-file > mysql.out
		```