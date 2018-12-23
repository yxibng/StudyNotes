# install 
```
brew install mysql
```

# start

```
brew services start mysql
```

# stop 

```
brew services stop mysql
```

# connect to mysql server

1. mysql 安装在别的主机
	
	```
	shell> mysql -h host -u user -p
	Enter password: ********
	```
2. mysql 安装在当前主机

	```
	shell> mysql -u user -p
	```

# disconnect
  
  ```
  mysql> QUIT
  Bye
  ```

# 修改密码

`ALTER USER 'user'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';`

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
```



