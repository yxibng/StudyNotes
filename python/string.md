1. 可以使用`''`或`""`, 可以用`\`来转义符号



	使用`''`
	
	```python
	>>> 'spam eggs'
	'spam eggs'
	
	```
	
	
	`''`内部有`'`使用`\`
	
	```python
	>>> 'doesn\'t'
	"doesn't"
	```
	
	`""`内部有`'`
	
	```python
	>>> "doesn't"
	"doesn't"
	```
	
	`''`内部有`""`
	
	```python
	>>> '"Yes,"they said.'
	'"Yes,"they said.'
	```
	
	`""`内部有`"`使用`\`
	
	```python
	>>> "\"Yes,\" they said"
	'"Yes," they said'
	```
	
	`''`内部有`""`,`""`内部有`'`以及`\`
	
	```python
	>>> '"Isn\'t," they said.'
	'"Isn\'t," they said.'
	```

2. 使用`print()`
	- 对比交互解释器输出, `print`会去掉输出字符串两端的引号(单引号或双引号)
	- `print(r'xxxx')`会输出字符串的原始格式
	- 多行输入`'''...'''` 或者 `""".."""`, 在行尾放`\`可以阻止换行

	
	```python
	>>> print('C:\some\name')  # here \n means newline!
	C:\some
	ame
	>>> print(r'C:\some\name')  # note the r before the quote
	C:\some\name
	```
	
	```python
	>>> print("""\
	... Usage: thingy [OPTIONS]
	...      -h                        Display this usage message
	...      -H hostname               Hostname to connect to
	... """)
	Usage: thingy [OPTIONS]
	     -h                        Display this usage message
	     -H hostname               Hostname to connect to
	```

3. 可以以使用`+`和`*`
	
	- `+`可以连接两个字符串
	- `*`使字符串重复n次

	```python
	>>> 3 *'un' + 'ium'
	'unununium'
	```
4. 两个字面值字符串相邻的时候会自动连接
	
	```python
	>>> 'Py' 'thon'
	'Python'
	```

	经常用在把一个长的字符串分割的时候
	
	```python
	>>> text = ('Put several strings within parentheses '
	...         'to have them joined together.')
	>>> text
	'Put several strings within parentheses to have them joined together.'
	```
	
	只支持字面值,不支持变量或者声明
	
	```python
	>>> prefix = 'Py'
	>>> prefix 'thon'  # can't concatenate a variable and a string literal
	  File "<stdin>", line 1
	    prefix 'thon'
	                ^
	SyntaxError: invalid syntax
	>>> ('un' * 3) 'ium'
	  File "<stdin>", line 1
	    ('un' * 3) 'ium'
	                   ^
	SyntaxError: invalid syntax
	```	

	可以通过`+`连接字面值和变量
	
	```python 
	>>> prefix + 'thon'
	'Python'
	```
	
4. 字符串下标
	
	- 下标从0开始
	- 可以使用负的下标,从`-1`开始
		
		-1 代表最后一个字符</br>
		-2 达标倒数第二个字符
	- 下标不能越界
	- 切片的时候下标可以越界
	
	
	```python
	>>> word = 'Python'
	>>> word[0]  # character in position 0
	'P'
	>>> word[5]  # character in position 5
	'n'
	>>> word[-1]  # last character
	'n'
	>>> word[-2]  # second-last character
	'o'
	>>> word[-6]
	'P'	
	```
	
	- 字符串切片`[x:y]`
		
		- x的默认值是0
		- y的默认值是字符串的长度
		- 切片的下标可以越界(上下界都可以)

	```python
	>>> word[0:2]  # characters from position 0 (included) to 2 (excluded)
	'Py'
	>>> word[2:5]  # characters from position 2 (included) to 5 (excluded)
	'tho'
	>>> word[:2]   # character from the beginning to position 2 (excluded)
	'Py'
	>>> word[4:]   # characters from position 4 (included) to the end
	'on'
	>>> word[-2:]  # characters from the second-last (included) to the end
	'on'
	```
	
	数据格式如下
	
	```
	 +---+---+---+---+---+---+
	 | P | y | t | h | o | n |
	 +---+---+---+---+---+---+
	 0   1   2   3   4   5   6
	-6  -5  -4  -3  -2  -1
	
	```
	
	
	越界的情况
	
	- 下标越界,取越界下标的字符,报错
	
		```python
		>>> word[42]  # the word only has 6 characters
		Traceback (most recent call last):
		  File "<stdin>", line 1, in <module>
		IndexError: string index out of range
		```
	- 如果是取切片,不报错


		```python
		>>> word[4:42]
		'on'
		>>> word[42:]
		''
		```
4. 字符串不可以修改,如果你需要不同的字符串,可以创建一个新的字符串
	
	```python
	>>> word[0] = 'J'
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	TypeError: 'str' object does not support item assignment
	>>> word[2:] = 'py'
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	TypeError: 'str' object does not support item assignment
	
	>>> 'J' + word[1:]
	'Jython'
	>>> word[:2] + 'py'
	'Pypy'
	```

5. 可以通过`len()`取字符串的长度.
	
	```python
	>>> s = 'supercalifragilisticexpialidocious'
	>>> len(s)
	34
	```
