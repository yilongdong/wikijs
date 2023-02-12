---
title: vcpkg安装使用
description: 
published: true
date: 2023-02-12T01:44:54.841Z
tags: 包管理, vcpkg
editor: markdown
dateCreated: 2023-02-12T01:44:54.841Z
---

# vcpkg安装使用
https://vcpkg.io/en/getting-started.html
```shell
git clone https://github.com/Microsoft/vcpkg.git
./vcpkg/bootstrap-vcpkg.sh -disableMetrics
cd vcpkg
export VCPKG_DIR="$(pwd)"
export VCPKG_CMAKE_PATH="$VCPKG_DIR/scripts/buildsystems/vcpkg.cmake"
```


`cmake -B [build directory] -S . -DCMAKE_TOOLCHAIN_FILE=[path to vcpkg]/scripts/buildsystems/vcpkg.cmake`

