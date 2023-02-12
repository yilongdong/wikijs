---
title: CMake常用检索
description: 
published: true
date: 2023-02-12T15:05:28.760Z
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

## 设置C++标准
```cmake
set(CMAKE_CXX_STANDARD 17)
target_compile_features(xxx PRIVATE cxx_std_17)
```
## cmake clean
```shell
cmake --build build/debug --target clean
```
[Looking for a 'cmake clean' command to clear up CMake output](https://stackoverflow.com/questions/9680420/looking-for-a-cmake-clean-command-to-clear-up-cmake-output)