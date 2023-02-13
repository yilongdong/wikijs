---
title: CMake结合Doxygen使用
description: 
published: true
date: 2023-02-13T06:48:35.642Z
tags: cmake, doxygen
editor: markdown
dateCreated: 2023-02-13T06:48:35.642Z
---

# CMake结合Doxygen使用

## find_package(Doxygen)
模块会定义两个变量
`DOXYGEN_FOUND`，找到为true
`DOXYGEN_VERSION`，doxygen版本

## doxygen_add_docs介绍
```cmake
doxygen_add_docs(targetName
    [filesOrDirs...]
    [ALL]
    [USE_STAMP_FILE]
    [WORKING_DIRECTORY dir]
    [COMMENT comment])
```
该函数构造一个 Doxyfile ,并定义一个在生成的文件上运行Doxygen的自定义目标。列出的文件和目录用作生成的 Doxyfile 的 INPUT ，它们可以包含通配符。明确列出的所有文件也将作为自定义目标的 SOURCES 添加，因此它们将显示在IDE项目的源列表中。

为了使相对输入路径按预期工作，默认情况下，Doxygen命令的工作目录将是当前源目录（即 `CMAKE_CURRENT_SOURCE_DIR` ）。可以使用 `WORKING_DIRECTORY` 选项将其覆盖，以更改用作相对基点的目录。
在调用 `doxygen_add_docs()` 之前通过设置CMake变量来定制生成的 Doxyfile 的内容。名称形式为 `DOXYGEN_<tag>` 的任何变量都将用其值替换 Doxyfile 中相应的 `<tag>` 配置选项。
## cmake文件
`DocForLibrary.cmake`文件
```cmake
find_package(Doxygen REQUIRED dot)
if (Doxygen_FOUND)
    set( DOXYGEN_GENERATE_HTML YES )
    set( DOXYGEN_GENERATE_MAN YES )
    set( DOXYGEN_OUTPUT_DIRECTORY doxygen )
    set( DOXYGEN_COLLABORATION_GRAPH YES )
    set( DOXYGEN_EXTRACT_ALL YES )
    set( DOXYGEN_CLASS_DIAGRAMS YES )
    set( DOXYGEN_HIDE_UNDOC_RELATIONS NO )
    set( DOXYGEN_HAVE_DOT YES )
    set( DOXYGEN_CLASS_GRAPH YES )
    set( DOXYGEN_CALL_GRAPH YES )
    set( DOXYGEN_CALLER_GRAPH YES )
    set( DOXYGEN_COLLABORATION_GRAPH YES )
    set( DOXYGEN_BUILTIN_STL_SUPPORT YES )
    set( DOXYGEN_EXTRACT_PRIVATE YES )
    set( DOXYGEN_EXTRACT_PACKAGE YES )
    set( DOXYGEN_EXTRACT_STATIC YES )
    set( DOXYGEN_EXTRACT_LOCALMETHODS YES )
    set( DOXYGEN_UML_LOOK YES )
    set( DOXYGEN_UML_LIMIT_NUM_FIELDS 50 )
    set( DOXYGEN_TEMPLATE_RELATIONS YES )
    set( DOXYGEN_DOT_GRAPH_MAX_NODES 100 )
    set( DOXYGEN_MAX_DOT_GRAPH_DEPTH 0 )
    set( DOXYGEN_DOT_TRANSPARENT YES )
    doxygen_add_docs(
            doxygen
            ${PROJECT_SOURCE_DIR}/lib ${PROJECT_SOURCE_DIR}/include
            COMMENT "Generate Library documentation"
    )
else()
    message("Doxygen need to be installed to generate the doxygen documentation")
endif()
```

## 使用
```cmake
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
include(DocForLibrary)
```

## 构建
```cmake
cmake --build build/release --target doxygen 
``