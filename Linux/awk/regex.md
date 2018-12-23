Regular Expression (正则表达式)
主要用来进行模式配对

- `.`匹配任意一个字符
- 匹配元字符需要转义
- `^`匹配字符串开头, 或者集合取反
- `$`匹配字符串结尾
- `[]`匹配集合中的任一个
- `*` 前面的字符可以出现 0次 或 多次
- `+` 前面的字符必须出现 1次 或 多次
- `?` 前面的字符可以出现 0次 或 1次
- `{}`前面的字符必须出现指定次数
- `()`把里面的内容视为一个整体
1. `/abc/`

```

cat data1.txt | awk '/abc/{print $0}' #或者 awk '/abc/{print $0}' data1.txt
abc
xxabc
xxabcxx
```
2. `/a.c/`

```
➜  awk git:(master) ✗ cat data1.txt | awk '/a.c/{print $0}'
abc
xxabc
xxabcxx
a.c
```
3. `/a\.c/` 只匹配 `a.c`

- `/a\/c/` - `/a/c/`
- `a\\c/` - `/a\c/`
- `/a\?c/` - `/a?c/`

4. `^` 和  `&`

```
➜  awk git:(master) ✗ cat data1.txt | awk '/^abc/{print $0}'
abc
➜  awk git:(master) ✗ cat data1.txt | awk '/abc$/{print $0}'
abc
xxabc
```

5. `[]`

`/a[xyz]c` 匹配
- `axc`
- `ayc`
- `azc`

`/a[a-z]c`匹配
- `/aac/` `/abc/` ...  `/ayc/` `/azc`

`/a[a-zA-Z]c` 匹配
- `/aac/` `/abc/` ...  `/ayc/` `/azc`
- `/aAc/` `/aBc/` ...  `/aYc/` `/aZc`


`/a[^a-z]c/` 匹配 
- 除了 中间 字符不是 `a-z`的字符串， 
- `^`在这里代表这取反操作


6.  `*` 和 `+`

`/a*b/` 匹配
- `b`
- `ab`
- `aab`
- `aaab`
- `aaaaaaab`

`a+b` 匹配
- `ab`
- `aab`
- `aaab`
- `aaaaaaab`

7. `?`

`/a?b/` 匹配
- `b`
- `ab`

8. `{}`

`/ab{3}c/`
- `abbbc`

`/ab{3, 4}c/`
- `abbbc`
- `abbbbc`

`/ab{3, 10}c/`
- `b`要出现 3-10次

`/ab{3, }c/`
- `b`要出现 3次或更多次

9. `()`

`/(ab)+c`
- `ab`至少出现1次





