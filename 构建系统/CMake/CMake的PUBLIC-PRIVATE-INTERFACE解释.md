---
title: CMake的PUBLIC-PRIVATE-INTERFACE解释
description: 
published: true
date: 2023-02-18T07:18:37.720Z
tags: cmake
editor: markdown
dateCreated: 2023-02-18T07:18:37.720Z
---

# CMake的PUBLIC、PRIVATE、INTERFACE解释
[原文](https://zhuanlan.zhihu.com/p/493493849)
```cmake
cmake_minimum_required(VERSION 3.16)

project(foobar)
set(CMAKE_SKIP_RPATH TRUE)

include_directories(${CMAKE_SOURCE_DIR})

add_library(foo SHARED foo.cpp)

add_library(bar SHARED bar.cpp)
target_link_libraries(bar PUBLIC foo)

add_executable(app app.cpp)
target_link_libraries(app bar)
```
## PRIVATE
bar.h没有包含foo.h，只有bar.cpp包含了foo.h。此时，app没有包含foo.h，因此不能使用foo中定义的符号。换句话说，app只知道bar的存在，完全不知道foo的存在。

## INTERFACE
bar.h中包含了foo.h，但是bar.cpp并没有用到foo定义的符号。此时，app包含了foo.h，可以引用foo的符号。换句话说，bar只是作为一个接口、界面，把foo传递给了app。

## PUBLIC
等于PRIVATE加INTERFACE，bar.h包含了foo.h，且bar.cpp引用了foo的符号。此时，app包含了foo.h，可以引用foo的符号。