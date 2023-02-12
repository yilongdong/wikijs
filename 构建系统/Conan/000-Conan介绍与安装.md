---
title: 000-Conan介绍与安装
description: 
published: true
date: 2023-02-12T00:39:57.851Z
tags: c++, conan, 包管理
editor: markdown
dateCreated: 2023-02-09T15:43:40.334Z
---

# Conan介绍与安装
https://docs.conan.io/en/latest/
## 安装

```shell
pip install conan
```

在 https://conan.io/center 搜索poco或者在命令行
```shell
conan search poco --remote=conancenter
conan inspect poco/1.9.4
```

```shell
mkdir build && cd build
conan install .. --build=missing
```