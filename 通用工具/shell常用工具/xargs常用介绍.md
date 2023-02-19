---
title: xargs常用介绍
description: 
published: true
date: 2023-02-19T12:37:29.093Z
tags: shell
editor: markdown
dateCreated: 2023-02-19T12:37:29.093Z
---

# xargs常用介绍
[xargs命令教程](https://www.ruanyifeng.com/blog/2019/08/xargs-tutorial.html)

## -d 参数与分隔符

默认情况下，xargs将换行符和空格作为分隔符，把标准输入分解成一个个命令行参数。
```shell
echo "one two three" | xargs mkdir
```
`-d`参数可以更改分隔符。
```shell
echo -e "a\tb\tc" | xargs -d "\t" echo
```

## -0 参数与 find 命令
由于xargs默认将空格作为分隔符，所以不太适合处理文件名，因为文件名可能包含空格。

find命令有一个特别的参数-print0，指定输出的文件列表以null分隔。然后，xargs命令的-0参数表示用null当作分隔符。
```shell
find /path -type f -print0 | xargs -0 rm
```

## -L 参数与 -n 参数
如果标准输入包含多行，-L参数指定多少行作为一个命令行参数。
```shell
echo -e "a\nb\nc" | xargs -L 1 echo
```
-L参数虽然解决了多行的问题，但是有时用户会在同一行输入多项。
```shell
$ echo {0..9} | xargs -n 2 echo
0 1
2 3
4 5
6 7
8 9
```

## -I 参数
如果xargs要将命令行参数传给多个命令，可以使用-I参数。

-I指定每一项命令行参数的替代字符串。
```shell
cat foo.txt | xargs -I file sh -c 'echo file; mkdir file'
```

## -p 参数，-t 参数
使用xargs命令以后，由于存在转换参数过程，有时需要确认一下到底执行的是什么命令。

-p参数打印出要执行的命令，询问用户是否要执行。

```shell
$ echo 'one two three' | xargs -p touch
touch one two three ?...
```
上面的命令执行以后，会打印出最终要执行的命令，让用户确认。用户输入y以后（大小写皆可），才会真正执行。

-t参数则是打印出最终要执行的命令，然后直接执行，不需要用户确认。

```shell
$ echo 'one two three' | xargs -t rm
rm one two three
```