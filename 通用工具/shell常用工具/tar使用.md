---
title: tar使用
description: 
published: true
date: 2023-03-04T02:38:04.608Z
tags: shell, tar
editor: markdown
dateCreated: 2023-03-04T02:38:04.608Z
---

# tar使用
tar命令详解

-c: 建立压缩档案

-x：解压

这两个是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但只能用其中一个。

下面的参数是根据需要在压缩或解压档案时可选的。

-z：有gzip属性的

-j：有bz2属性的

-Z：有compress属性的

-v：显示所有过程

参数-f是必须的

-f: 使用档案名字，这个参数是最后一个参数，后面只能接档案名。

```shell
tar -xf cmake-3.25.2.tar.gz
```