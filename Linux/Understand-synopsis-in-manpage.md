参考

- [https://unix.stackexchange.com/questions/17833/understand-synopsis-in-manpage](https://unix.stackexchange.com/questions/17833/understand-synopsis-in-manpage)
- [http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html#tag_12](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html#tag_12)


在看man page 的时候,经常看到下面的语法

```
utility_name[-a][-b][-c option_argument]
    [-d|-e][-f[option_argument]][operand...]
```
例如执行 

``` bash
man sed
```

```bash
NAME
     sed -- stream editor

SYNOPSIS
     sed [-Ealn] command [file ...]
     sed [-Ealn] [-e command] [-f command_file] [-i extension] [file ...]
```


## 这种语法该如何解读?

1. `utility_name` 代表命令的名字,后面是可选项`options `,可选项对应的参数`option-arguments`,操作对象 `operands`
	- 以连字符连接的单个字符,成为 选项 `options `,以前被称为`flags `
	- 某些选项之后可以接参数,例如 `[-c option_argument]`
	- 排在最后一个选项和选项参数之后的,出现在末尾的参数,是被操作对象`operands `
2. 选项参数和选项
 	- 指定了参数的选项,参数是必须的  `[ -c option_argument]`, 选项和参数之间用`空格`分隔
 	
	 	```
	 	utility_name -c option_argument
	 	```
 	- 如果 参数在`[]`内,参数是可选的  `[ -f[ option_argument]]`. 但是此时如果指定了参数, 选项和参数之间没有空格,直接连接到一起,避免后面的内容被当做了当前选项的参数

	 	```
	 	utility_name -c option_argument -f
	 	utility_name -c option_argument -foption_argument
	 	```
	 	

3. 选项一般是按照字母表顺序给出的
4. 包含在`<parameter name>`里面的参数要被替换成具体的值,替换之后去掉`< >`
5. 当一个命令有多个不需要参数的选项时,可以把选项放到一起
	 
	 ```
	 utility_name [-abcDxyz][-p arg][operand]
	 ```
6. 除非特别指定,选项参数`option_argument `和操作对象`operand `中 包含或者就是数字 的情况下
	- 这个数字会被解释为十进制数
	- 数的范围 `0 - 2147483647`会被认为是数字
	- 接受负数的情况,范围 ` -2147483647` - `2147483647`会被认为是数字
7. 参数和可选参数
	- `[]`括起来就代表是可选择的
8. `|`分割的选项是互斥的,只能存在其中的一个.在`synopsis`中互斥的选项可以被展示为多行

	```
	utility_name -d[-a][-c option_argument][operand...]
	utility_name[-a][-b][operand...]
	```
9. `...` 代表可以存在多个操作对象`operand ` 或者同样的选项` option`可以出现多次
	
	```
	utility_name [-g option_argument]...[operand...]
	```
	
## 总结
1. `[]`代表可选,区分`[-c option_argument]`, `[-f[option_argument]]`
	- `-c option_argument` 空格分隔
	- `-foption_argument ` 没有空格,直接相连
2. `<>`里面的内容要被替换,替换后,不保留`<>`
3. `|`代表互斥,只能存在一个
4. `...`代表可以存在多个
	
	 















