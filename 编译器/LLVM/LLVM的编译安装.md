---
title: LLVM的编译安装
description: 
published: true
date: 2023-02-12T01:22:54.744Z
tags: 编译器, llvm
editor: markdown
dateCreated: 2023-02-12T01:22:54.744Z
---

# LLVM的编译安装
```shell
git clone https://github.com/llvm/llvm-project.git llvm-project

git branch -a

git checkout remotes/origin/release/15.x

cmake -G Ninja -DDEFAULT_SYSROOT="$(xcrun --show-sdk-path)" -DCMAKE_BUILD_TYPE=Release -DLLVM_TARGETS_TO_BUILD=host -DLLVM_ENABLE_PROJECTS="clang;libcxx;libcxxabi" ../llvm

ninja install
```