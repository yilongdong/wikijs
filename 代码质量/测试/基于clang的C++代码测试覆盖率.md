---
title: 基于clang的C++代码测试覆盖率
description: 
published: true
date: 2023-02-09T15:29:29.050Z
tags: clang, c++, 测试覆盖率, 代码质量
editor: markdown
dateCreated: 2023-02-09T15:29:29.050Z
---

# 基于clang的C++代码测试覆盖率

https://clang.llvm.org/docs/SourceBasedCodeCoverage.html#creating-coverage-reports

https://blog.csdn.net/u012247418/article/details/90137291

## gcov+lcov

### 编译
```shell
clang++ -fprofile-arcs -ftest-coverage main.cpp -o main
```
会生成main.gcno和main文件
### 执行收集信息，产生xxx.gcda


```shell
./main
```
会生成main.gcda文件

### 使用gcov生成报告
```shell
gcov main.cpp
```
这会创建main.cpp.gcov文件。此时是txt文件，可读性较差。

### 使用lcov生成相应信息并进行web可视化
```shell
lcov -d . -o main.info -b . -c
genhtml -o result main.info
open result/index.html
```
![image-20230209232752626](https://yilongdong-blog.oss-cn-hangzhou.aliyuncs.com/img/image-20230209232752626.png)

## llvm-profdata + llvm-cov

https://clang.llvm.org/docs/SourceBasedCodeCoverage.html#creating-coverage-reports
