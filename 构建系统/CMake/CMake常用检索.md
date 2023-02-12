---
title: CMake常用检索
description: 
published: true
date: 2023-02-12T15:01:23.495Z
tags: cmake
editor: markdown
dateCreated: 2023-02-12T15:01:23.495Z
---

# CMake常用检索

## 使用vcpkg编译项目
```shell
cmake -B build/debug -S . -G Ninja -DCMAKE_BUILD_TYPE=Debug -DCMAKE_TOOLCHAIN_FILE=$(dirname $(which vcpkg))/scripts/buildsystems/vcpkg.cmake
```
```shell
cmake --build build/debug --parallel
```