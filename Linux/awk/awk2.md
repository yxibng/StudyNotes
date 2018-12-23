输出第7行
```
awk 'NR==7{print NR, $0}' coins.txt
7 gold      0.1  1986    PRC                 Panda
```

输出有7列数据的记录

```
awk 'NF==7{print NR, $0}' coins.txt

9 gold      0.25 1986    USA                 Liberty 5-doller piece
10 silver    0.5  1986    USA                 Liberty 50-cent piece
11 silver    1    1987    USA                 Constitution doller piece
```

接受标准输入

```
awk '{print $1, $2}'

hello world
hello world
a b c
a b

awk '{print $2, $1}'

hello world
world hello
a b c
b a

```

- awk 接受输入的默认分割符号是空格
- 输入分隔符`FS` = input Field seperator
- 修改默认的分割符的方法是通过定义新的变量，通过  `BEGIN{FS="xx"}`的方式
- 输出分隔符 `OFS` = output field seperator

```
awk '{print $2, $1}'

a b c
b a
hello,world 123 456
123 hello,world
```

修改默认输入的分割符为`,`

```
awk 'BEGIN{FS=","}{print $1, $2}'

a b c
a b c
hello world
hello world
heelo,world,123
heelo world

```
修改输出的分隔符

```
awk 'BEGIN{OFS=","}{print $1, $2}'
hello world
hello,world
```

同时设置输入和输出的分隔符

```
awk 'BEGIN{FS=",";OFS="\t"}{print $1, $2}'
hello,world
hello   world
```


多个文件作为输入的情况

```
awk '{print NR, $0}' coins.txt data.txt

1 gold      1    1986    USA                 American Eagle
2 gold      1    1908    Austria-Hungary     Franz Josef 100 Korona
3 silver    10   1981    USA                 Ingot
4 gold      1    1984    Switzerland         ingot
5 gold      1    1979    RSA                 Krugerrand
6 gold      0.5  1981    RSA                 Krugerrand
7 gold      0.1  1986    PRC                 Panda
8 silver    1    1986    Liberty             doller
9 gold      0.25 1986    USA                 Liberty 5-doller piece
10 silver    0.5  1986    USA                 Liberty 50-cent piece
11 silver    1    1987    USA                 Constitution doller piece
12 gold      1    1988    Canda               Leaf
13 hello world
14 apple banana orange
15 abcd efgh
```

输出文件名
- `FILENAME` 文件名

```
awk '{print NR,FILENAME, $0}' coins.txt data.txt

1 coins.txt gold      1    1986    USA                 American Eagle
2 coins.txt gold      1    1908    Austria-Hungary     Franz Josef 100 Korona
3 coins.txt silver    10   1981    USA                 Ingot
4 coins.txt gold      1    1984    Switzerland         ingot
5 coins.txt gold      1    1979    RSA                 Krugerrand
6 coins.txt gold      0.5  1981    RSA                 Krugerrand
7 coins.txt gold      0.1  1986    PRC                 Panda
8 coins.txt silver    1    1986    Liberty             doller
9 coins.txt gold      0.25 1986    USA                 Liberty 5-doller piece
10 coins.txt silver    0.5  1986    USA                 Liberty 50-cent piece
11 coins.txt silver    1    1987    USA                 Constitution doller piece
12 coins.txt gold      1    1988    Canda               Leaf
13 data.txt hello world
14 data.txt apple banana orange
15 data.txt abcd efgh
```

输出的时候，修改某一列

```
awk '{$3="xxxx";print "\t" NR "\t" $0}' coins.txt

  1     gold 1 xxxx USA American Eagle
  2     gold 1 xxxx Austria-Hungary Franz Josef 100 Korona
  3     silver 10 xxxx USA Ingot
  4     gold 1 xxxx Switzerland ingot
  5     gold 1 xxxx RSA Krugerrand
  6     gold 0.5 xxxx RSA Krugerrand
  7     gold 0.1 xxxx PRC Panda
  8     silver 1 xxxx Liberty doller
  9     gold 0.25 xxxx USA Liberty 5-doller piece
  10    silver 0.5 xxxx USA Liberty 50-cent piece
  11    silver 1 xxxx USA Constitution doller piece
  12    gold 1 xxxx Canda Leaf
```

打印最后一列

```
awk '{$3="xxxx";print $NF}' coins.txt
Eagle
Korona
Ingot
ingot
Krugerrand
Krugerrand
Panda
doller
piece
piece
piece
Leaf
```

打印倒数第二列

```
awk '{$3="xxxx";print $(NF-1)}' coins.txt

American
100
USA
Switzerland
RSA
RSA
PRC
Liberty
5-doller
50-cent
doller
Canda
```

可以声明语句,进行计算操作,但是要注意运算的优先级,记得按回车

```
awk '{a=1; b=3; print a + b}'

4

#此时是字符串连接的
awk '{a=1; b=3; print a  b}'

13

#此时先计算再连接
awk '{a=1; b=2; c=3;  print a b+c}'

15

#字符转数字失败，默认值设置为0
awk '{a=1; b="apple"; c=3;  print a b+c}'

13

#数字出现在开头，部分强转
awk '{a=1; b="56apple"; c=3;  print a b+c}'

159

#数字出现在末尾，强转失败
awk '{a=1; b="apple56"; c=3;  print a b+c}'

13
```







