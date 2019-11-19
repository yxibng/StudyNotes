# s - substitude 替换

```bash
$ echo day | sed s/day/night/
night

$ echo Sunday | sed 's/day/night/'
Sunnight

```

### sed is line oriented

默认只会更改每行的第一个匹配结果

```bash
$ cat file
one two three, one two three
four three two one
one hundred

$ sed 's/one/ONE/' <file
ONE two three, one two three
four three two ONE
ONE hundred

```

使用`/g`进行全局替换

```bash
$ cat file | sed 's/one/ONE/g'
ONE two three, ONE two three
four three two ONE
ONE hundred
```

使用`/1,/2,etc.`指定匹配出现的位置


```bash
$ cat file | sed 's/one/ONE/1'
ONE two three, one two three
four three two ONE
ONE hundred

$ cat file | sed 's/one/ONE/2'
one two three, ONE two three
four three two one
one hundred
```

多个sed命令相连可以使用`-e`

```bash
$ cat file | sed -e 's/one/ONE/' -e 's/two/TWO/'
ONE TWO three, one two three
four three TWO ONE
ONE hundred
```


在sed脚本里可以使用变量

```bash
var=one

cat file | sed 's/'$var'/ONE/'
ONE two three, one two three
four three two ONE
ONE hundred
```






### Delimiter

There are four parts to this substitute command:

```
s	  Substitute command
/../../	  Delimiter
one	  Regular Expression Pattern Search Pattern
ONE	  Replacement string

```

1. `/../../`为分隔符, 替换字符包含分隔符,需要转义

    ```bash
    sed 's/\/usr\/local\/bin/\/common\/bin/' <old >new
    ```
2. 自定义分隔符

    使用`_`做分隔符
    ```bash
    sed 's_/usr/local/bin_/common/bin_' <old >new
    ```
    使用`:`做分隔符
    ```bash
    sed 's:/usr/local/bin:/common/bin:' <old >new
    ```
    使用`|`做分隔符
    ```bash
    sed 's|/usr/local/bin|/common/bin|' <old >new
    ```

# Using & as the matched string

使用`&`可以进行模糊查询, 可以使用多个`&`,将匹配到的模式翻倍

```bash
$ echo "123 abc" | sed 's/[0-9]*/&/'      
123 abc

$ echo "123 abc" | sed 's/[0-9]*/& &/'
123 123 abc
```


# 指定匹配的范围

### 指定匹配的行

删除第一行的one

```bash
$ cat file | sed '1 s/one//'
 two three, one two three
four three two one
one hundred
```

删除第二行的two

```bash
$ cat file | sed '2 s/two//'
one two three, one two three
four three  one
one hundred
```

删除第1-3行的one
```
$ cat file | sed '1,3 s/one//'
 two three, one two three
four three two
 hundred
```

### 使用模式匹配 ```sed '/start/,/stop/ s/#.*//'```

匹配第一行到包含`four`那一行,替换`one`为`ONE`
```bash
$ cat file | sed '1,/four/ s/one/ONE/'
ONE two three, one two three
four three two ONE
one hundred
```
匹配以包含`one`开头,到包含`hundred`结束的行,替换`one`为`ONE`
```bash
$ cat file | sed '/two/,/hundred/ s/one/ONE/'
ONE two three, one two three
four three two ONE
ONE hundred
```


匹配包行`hundred`的行,将`one`替换为`ONE`

```bash
$ cat file | sed '/hundred/ s/one/ONE/'
one two three, one two three
four three two one
ONE hundred
```

# d - Delete

删除第一行

```bash
$ cat file | sed '1 d'
four three two one
one hundred
```

删除2-3行

```bash
$ cat file | sed '2,3 d'
one two three, one two three
```

从第2行开始删除到末尾

```bash
$ cat file | sed '2,$ d'
one two three, one two three
```


# a - Append 追加在行后

在包含`one`的行后追加一行


```bash
$ cat file | sed '/one/ a\
pipe quote> add'
one two three, one two three
addfour three two one
addone hundred
add%


$ cat file | sed '/one/ a\
add
pipe quote> '
one two three, one two three
add
four three two one
add
one hundred
add

```

# i - Insert 插入在行前
```bash
$ cat file | sed '/one/ i\
pipe quote> inserted content!!!
pipe quote> '
inserted content!!!
one two three, one two three
inserted content!!!
four three two one
inserted content!!!
one hundred
```

# c - Change 替换一整行

将包含`hundred`的行替换

```bash
$ cat file | sed '/hundred/ c\
pipe quote> replaced by change command!!!
pipe quote> '
one two three, one two three
four three two one
replaced by change command!!!
```

将包含`one`的行替换
```bash
$ cat file | sed '/one/ c\
replaced by change command!!!
'
replaced by change command!!!
replaced by change command!!!
replaced by change command!!!
```
#  查找字符，替换为换行符
```bash
sed 's/regexp/\'$'\n/g'
```
