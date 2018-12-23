xargs 

从文件或者标准输入中接收数据，将数据作为命令的参数多次执行。

1. 将输入变成以空格分隔的一系列参数,多行变一行
```
→ ps -al
F S   UID   PID  PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
4 S     0  1551  1549  0  80   0 - 193070 -     tty1     00:31:31 Xorg
0 S  1000  1618  1549  0  80   0 - 141954 poll_s tty1    00:00:01 gnome-session-b
0 R  1000  1757  1618  1  80   0 - 1288014 -    tty1     02:23:47 gnome-shell
0 S  1000  2117  1757  0  80   0 - 91776 poll_s tty1     00:05:04 ibus-daemon
0 S  1000  2121  2117  0  80   0 - 70187 poll_s tty1     00:00:00 ibus-dconf
0 S  1000  2123     1  0  80   0 - 86160 poll_s tty1     00:00:04 ibus-x11
0 S  1000  2319  1618  0  80   0 - 129463 poll_s tty1    00:00:01 gsd-power

→ ps -al | xargs
F S UID PID PPID C PRI NI ADDR SZ WCHAN TTY TIME CMD 4 S 0 1551 1549 0 80 0 - 193070 - tty1 00:31:32 Xorg 0 S 1000 1618 1549 0 80 0 - 141954 poll_s tty1 00:00:01 gnome-session-b 0 S 1000 1757 1618 1 80 0 - 1288014 poll_s tty1 02:23:51 gnome-shell 0 S 1000 2117 1757 0 80 0 - 91776 poll_s tty1 00:05:05 ibus-daemon 0 S 1000 2121 2117 0 80 0 - 70187 poll_s tty1 00:00:00 ibus-dconf 0 S 1000 2123 1 0 80 0 - 86160 poll_s tty1 00:00:04 ibus-x11 0 S 1000 2319 1618 0 80 0 - 129463 poll_s tty1 00:00:01 gsd-power 0 S 1000 2321 1618 0 80 0 - 87329 poll_s tty1 00:00:0
```

2. 一行变多行,一次处理n个参数，经常用 `-n 1`

```
ps -al | xargs | xargs -n 5 
F S UID PID PPID
C PRI NI ADDR SZ
WCHAN TTY TIME CMD 4
S 0 1551 1549 0
80 0 - 193070 -
tty1 00:31:36 Xorg 0 S

```

3. 一次处理n行,常用 `-L 1`

```
→ ps -al | xargs -L 1
F S UID PID PPID C PRI NI ADDR SZ WCHAN TTY TIME CMD
4 S 0 1551 1549 0 80 0 - 193070 - tty1 00:31:39 Xorg
0 S 1000 1618 1549 0 80 0 - 141954 poll_s tty1 00:00:01 gnome-session-b
0 S 1000 1757 1618 1 80 0 - 1311628 poll_s tty1 02:24:33 gnome-shell
0 S 1000 2117 1757 0 80 0 - 91776 poll_s tty1 00:05:09 ibus-daemon

→ ps -al | xargs -L 2
F S UID PID PPID C PRI NI ADDR SZ WCHAN TTY TIME CMD 4 S 0 1551 1549 0 80 0 - 193070 - tty1 00:31:40 Xorg
0 S 1000 1618 1549 0 80 0 - 141954 poll_s tty1 00:00:01 gnome-session-b 0 R 1000 1757 1618 1 80 0 - 1311628 - tty1 02:24:36 gnome-shell
0 S 1000 2117 1757 0 80 0 - 91776 poll_s tty1 00:05:09 ibus-daemon 0 S 1000 2121 2117 0 80 0 - 70187 poll_s tty1 00:00:00 ibus-dconf
0 S 1000 2123 1 0 80 0 - 86160 poll_s tty1 00:00:04 ibus-x11 0 S 1000 2319 1618 0 80 0 - 129463 poll_s tty1 00:00:01 gsd-power

```

4. `-d`可以自定义定界符 

```
echo "nameXnameXnameXname" | xargs -dX

name name name name
```
结合-n选项使用：

```
echo "nameXnameXnameXname" | xargs -dX -n2

name name
name name
```
5. xargs的一个选项-I，使用-I指定一个替换字符串{}，这个字符串在xargs扩展时会被替换掉，当-I与xargs结合使用，每一个参数命令都会被执行一次：

```
→ cat test 
abc efg
123
456

→ cat test | xargs -I {} echo {}   
abc efg
123
456

→ cat test | xargs -I {} echo x+{}
x+abc efg
x+123
x+456



```


经过验证有`-I`选项的时候, `-L -n`的选项都不起作用，只会按行读取，`{}`替换的每一行的输入。

6. `-a`可以从文件输入

```
→ xargs -a test 
abc efg 123 456

→ xargs -a test -n 1
abc
efg
123
456

→ xargs -a test -L 1
abc efg
123
456

```