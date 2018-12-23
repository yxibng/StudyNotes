```
usage: awk [-F fs] [-v var=value] [-f progfile | 'prog'] [file ...]
```

- `$0` 代表文本文件本身
- `NR` = number of reocrd
- `NF` = number of field
- `record` = one row
- `field` =  one column
- `,` 代表用空格相连
- ` ` 空格代表直接相连


输出全部

```
awk '{print}' coins.txt
awk '{print $0}' coins.txt


gold      1    1986    USA                 American Eagle
gold      1    1908    Austria-Hungary     Franz Josef 100 Korona
silver    10   1981    USA                 Ingot
gold      1    1984    Switzerland         ingot
gold      1    1979    RSA                 Krugerrand
gold      0.5  1981    RSA                 Krugerrand
gold      0.1  1986    PRC                 Panda
silver    1    1986    Liberty             doller
gold      0.25 1986    USA                 Liberty 5-doller piece
silver    0.5  1986    USA                 USA Liberty 50-cent piece
silver    1    1987    USA                 Constitution doller
gold      1    1988    Canda               Leaf
```

打印第一列

```
awk '{print $1}' coins.txt
gold
gold
silver
gold
gold
gold
gold
silver
gold
silver
silver
gold
```


第1， 2， 3列

```
awk '{print $1, $2, $3}' coins.txt

gold 1 1986
gold 1 1908
silver 10 1981
gold 1 1984
gold 1 1979
gold 0.5 1981
gold 0.1 1986
silver 1 1986
gold 0.25 1986
silver 0.5 1986
silver 1 1987
gold 1 1988
```


输出格式美化,用制表符

```
awk '{print $1 "\t" $2 "\t " $3}' coins.txt
gold    1        1986
gold    1        1908
silver  10       1981
gold    1        1984
gold    1        1979
gold    0.5      1981
gold    0.1      1986
silver  1        1986
gold    0.25     1986
silver  0.5      1986
silver  1        1987
gold    1        1988
```

打印行号

```
awk '{print NR "\t" $1 "\t" $2 "\t " $3}' coins.txt
1       gold    1        1986
2       gold    1        1908
3       silver  10       1981
4       gold    1        1984
5       gold    1        1979
6       gold    0.5      1981
7       gold    0.1      1986
8       silver  1        1986
9       gold    0.25     1986
10      silver  0.5      1986
11      silver  1        1987
12      gold    1        1988
```

空格代表直接相连,`$0`输出文件本身
```
awk '{print NR $0}' coins.txt
1gold      1    1986    USA                 American Eagle
2gold      1    1908    Austria-Hungary     Franz Josef 100 Korona
3silver    10   1981    USA                 Ingot
4gold      1    1984    Switzerland         ingot
5gold      1    1979    RSA                 Krugerrand
6gold      0.5  1981    RSA                 Krugerrand
7gold      0.1  1986    PRC                 Panda
8silver    1    1986    Liberty             doller
9gold      0.25 1986    USA                 Liberty 5-doller piece
10silver    0.5  1986    USA                 USA Liberty 50-cent piece
11silver    1    1987    USA                 Constitution doller
12gold      1    1988    Canda               Leaf
```

用制表符分割,输出行号

```
awk '{print "\t" NR "\t" $0}' coins.txt
  1     gold      1    1986    USA                 American Eagle
  2     gold      1    1908    Austria-Hungary     Franz Josef 100 Korona
  3     silver    10   1981    USA                 Ingot
  4     gold      1    1984    Switzerland         ingot
  5     gold      1    1979    RSA                 Krugerrand
  6     gold      0.5  1981    RSA                 Krugerrand
  7     gold      0.1  1986    PRC                 Panda
  8     silver    1    1986    Liberty             doller
  9     gold      0.25 1986    USA                 Liberty 5-doller piece
  10    silver    0.5  1986    USA                 USA Liberty 50-cent piece
  11    silver    1    1987    USA                 Constitution doller
  12    gold      1    1988    Canda               Leaf
```

输出列数

```
awk '{print "\t" NF "\t" $0}' coins.txt

  6     gold      1    1986    USA                 American Eagle
  8     gold      1    1908    Austria-Hungary     Franz Josef 100 Korona
  5     silver    10   1981    USA                 Ingot
  5     gold      1    1984    Switzerland         ingot
  5     gold      1    1979    RSA                 Krugerrand
  5     gold      0.5  1981    RSA                 Krugerrand
  5     gold      0.1  1986    PRC                 Panda
  5     silver    1    1986    Liberty             doller
  7     gold      0.25 1986    USA                 Liberty 5-doller piece
  8     silver    0.5  1986    USA                 USA Liberty 50-cent piece
  6     silver    1    1987    USA                 Constitution doller
  5     gold      1    1988    Canda               Leaf
```

查找功能

```
awk '$3=1986{print $0}' coins.txt

gold 1 1986 USA American Eagle
gold 1 1986 Austria-Hungary Franz Josef 100 Korona
silver 10 1986 USA Ingot
gold 1 1986 Switzerland ingot
gold 1 1986 RSA Krugerrand
gold 0.5 1986 RSA Krugerrand
gold 0.1 1986 PRC Panda
silver 1 1986 Liberty doller
gold 0.25 1986 USA Liberty 5-doller piece
silver 0.5 1986 USA USA Liberty 50-cent piece
silver 1 1986 USA Constitution doller
gold 1 1986 Canda Leaf
```

需要注意字符串查找

```
awk '$1=gold{print $0}' coins.txt
```

not work,应该是下面的语句

```
awk '$1=="gold"{print $0}' coins.txt

gold 1 1986 USA American Eagle
gold 1 1908 Austria-Hungary Franz Josef 100 Korona
gold 10 1981 USA Ingot
gold 1 1984 Switzerland ingot
gold 1 1979 RSA Krugerrand
gold 0.5 1981 RSA Krugerrand
gold 0.1 1986 PRC Panda
gold 1 1986 Liberty doller
gold 0.25 1986 USA Liberty 5-doller piece
gold 0.5 1986 USA USA Liberty 50-cent piece
gold 1 1987 USA Constitution doller
gold 1 1988 Canda Leaf
```

查找 `$4="PRC"`

```
awk '$4=="PRC"{print $0}' coins.txt

gold      0.1  1986    PRC                 Panda
```












