---
title: vcpkg安装使用
description: 
published: true
date: 2023-02-12T09:22:49.131Z
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
pwd
echo 'export VCPKG_DIR="vcpkg_install_path"' >> ~/.zshrc
source ~/.zshrc
echo 'export VCPKG_CMAKE_PATH="$VCPKG_DIR/scripts/buildsystems/vcpkg.cmake"' >> ~/.zshrc
echo 'export PATH=$VCPKG_DIR:$PATH' >> ~/.zshrc
source ~/.zshrc
```

`cmake -B [build directory] -S . -DCMAKE_TOOLCHAIN_FILE=[path to vcpkg]/scripts/buildsystems/vcpkg.cmake`

## manifest模式
[manifest官方文档](https://learn.microsoft.com/en-us/vcpkg/examples/manifest-mode-cmake)

### 编写vcpkg.json文件
`vcpkg search xxx`一般不会列出历史版本。

可以使用`vcpkg x-history xxx`搜索可用的历史版本。

```json
{
  "name": "fibo",
  "version": "0.1.0",
  "dependencies": [
    "cxxopts",
    "fmt",
    "range-v3",
    {
      "name": "spdlog",
      "version>=": "1.11.0"
    }
  ],
  "builtin-baseline": "36fb23307e10cc6ffcec566c46c4bb3f567c82c6",
  "overrides": [
    {
      "name": "fmt",
      "version": "7.1.0"
    }
  ]
}
```

- `name`, `version`是项目名和版本。
- `dependencies`是依赖包，里面可以设置版本的一些限制
- `builtin-baseline`是使用的vcpkg的commit id，搜索包就是先把vcpkg切换到对应到commit，然后搜索包。
- `overrides`里面可以设置某个包的具体版本要求。

### 编译
```shell
cmake -B build -S . -G Ninja \
	-DCMAKE_BUILD_TYPE=Release \
	-DCMAKE_TOOLCHAIN_FILE=$VCPKG_CMAKE_PATH  
cmake --build build --parallel
```