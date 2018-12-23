# [UNIX SHELL Quote Tutorial](http://www.grymoire.com/Unix/Quote.html#TOC)


## 使用 `\`

- 放在一个特殊字符前,防止转义
	
	删除文件名为`*`的文件

	```bash
	rm -i \*
	```
	
- 放在行尾,多行语句输入
	
	```bash
	$ echo This could be \
	a very\
	long line \!
	This could be a verylong line !
	```

## 使用`''`强引用

输出a&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;b, 中间七个空格

1. 采用`\`

	```bash
	$ echo a\ \ \ \ \ \ \ b
	a       b
	```
2. 使用`''`
	
	```bash
	$ echo 'a       b'
	a       b
	```
3. 使用`''`基本可以包含所有的元字符
	
	```bash
	$ echo 'What the *heck* is a $ doing here???'
	What the *heck* is a $ doing here???
	```
	
## 使用`""`弱引用
弱引用: 不展开`*`或者 `?`等元字符,但是会展开变量和命令替换.

```bash
$ echo "Is your home directory $HOME?"
Is your home directory /Users/yaoxiaobing?

$ echo "Your current directory is `pwd`"
Your current directory is /Users/yaoxiaobing
```
引用强度对比
> '\' > `''` > `""`

## 使用`''`处理文件中有空格或者特殊字符的情况
如果你想处理名字中有空格或者特殊字符的文件,可以使用引用.

1. 创建一个名字中有空格的文件

	```bash
	$ touch 'a b'
	$ ls | grep 'a b'
	a b
	```
	
	可以使用`\`

	```bash
	$ rm a\ b
	```
2. 处理名字中有特殊字符的
 
 ```bash
 touch 'a?'
 $ ls | grep 'a?'
 a?	
 ```
## 引号里面有引号的情况

可以把`''` 和 `""`之一放到另一种之间.

- 要想引用`''`,放入`""`中
- 要想引用`""`,放入`''`中 


## 验证你的引号是否正确

> 有时候,你需要使用`\`,但是不确定自己的用法是否正确.此时可以使用shell去验证自己的用法.


## 在引号里包含同级别的引号

看两个示例

```bash
echo "The word for today is \"TGIF\""
echo 'Don\'t quote me'
```
输出

```bash
$ echo "The word for today is \"TGIF\""
The word for today is "TGIF"

$ echo 'Don\'t quote me'
quote>
```

为什么第二条语句是错的呢? 因为区别于C等语言,引号的作用只是打开和关闭替换.并不标识着字符串的开始和结束.

因此第二条语句分解如下:

```bash
echo 'Dont\'
t quote me
'
```



看下面的例子

```bash
$ echo 'a'b'c'
abc
```
分解如下

```bash
echo 'a'\
b\
'c'\
```
中间的部分还可以是一个变量

```bash
echo 'a'$HOME'b'
```
 
> If you want to include a single quote in an argument that starts with a single quote, you must turn off the mechanism started by the single quote, and use a different quoting method. Remember, the backslash is the strongest of all quoting mechanisms. You can quote anything with the backslash.

- 以`'`开头的的语句中使用`'`,必须先将这个`'`的区域结束,使用不同的引用级别来引用这个包含的`'`.
- `\`语义最强,可以引用任何内容
- 引号是配对使用的

1. 输出三中引用引用类型的字符
	
	```bash
	$ echo \'\"\\
	'"\
	```
2. 你总是可以使用`\`取引用一个字符,但是在`'`里面`\`不起作用.正确的方法是
	
	```bash
	$ echo 'Don'   \'   't do that'
	Don ' t do that
	
	```

3. 去掉上一句多余的空格
	
	```bash
	$ echo 'Don'\''t do that'
	Don't do that
	```
4. 记住引号是配对使用的.这对`""`也同样适用
	
	```bash
	$ echo "The quote for today is "\"TGIF\"
	The quote for today is "TGIF"
	```
	或者
	
	```bash
	$ echo "The quote for today is "\""TGIF"\"
	The quote for today is "TGIF"
	```


## 长引用
1. 使用`\`引用一个长句子
	
	```bash
	$ echo "A quote\
	dquote> on two lines"
	A quoteon two lines
	```
2. 有些shell支持多行引用

	```
	$ echo "A quote
	dquote> on two lines"
	A quote
	on two lines
	```






















