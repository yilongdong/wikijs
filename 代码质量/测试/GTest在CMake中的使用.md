---
title: GTest在CMake中的使用
description: 
published: true
date: 2023-02-13T01:38:43.184Z
tags: cmake, 测试, gtest
editor: markdown
dateCreated: 2023-02-13T01:38:43.184Z
---

# GTest在CMake中的使用
## add_test
`add_test`是CMake原生测试支持
```cmake
include(CTest)
enable_testing()

add_test(NAME <name> COMMAND <command> [<args>...]
				 [CONFIGURATIONS <config>...]
         [WORKING_DIRECTORY <dir>]
         [COMMAND_EXPAND_LISTS])
```
### 使用示例
```cmake
enable_testing()

add_executable(test_example test.cpp)
target_link_libraries(test_example example_lib)

add_test(NAME test_example1 COMMAND test_example --arg1=a --arg2=b)
add_test(NAME test_example2 COMMAND test_example --arg1=c --arg2=d)
```
### 运行测试用例
有三种方式
- `make test`
- `cmake --build . --target test`
- `ctest`

## gtest_add_tests

```cmake
gtest_add_tests(TARGET target
                [SOURCES src1...]
                [EXTRA_ARGS arg1...]
                [WORKING_DIRECTORY dir]
                [TEST_PREFIX prefix]
                [TEST_SUFFIX suffix]
                [SKIP_DEPENDENCY]
                [TEST_LIST outVar]
)
```
它是用来取代 `add_test` 的，通过扫描源代码，它就能读出所有的测试用例，省却了两边重复写的问题，但是它有个问题：一旦测试用例改变，它就需要重新跑 cmake，不然无法知道改变后的测试用例。

## gtest_discover_tests

```cmake
gtest_discover_tests(target
                     [EXTRA_ARGS arg1...]
                     [WORKING_DIRECTORY dir]
                     [TEST_PREFIX prefix]
                     [TEST_SUFFIX suffix]
                     [NO_PRETTY_TYPES] [NO_PRETTY_VALUES]
                     [PROPERTIES name1 value1...]
                     [TEST_LIST var]
                     [DISCOVERY_TIMEOUT seconds]
)
```
相比较于 gtest_add_tests，gtest_discover_tests 是通过获取编译后的可执行程序里面的测试用例来达到注册的目的，因此会更鲁棒，在测试用例改变的情况下，不需要重新运行 cmake
### 使用示例
```cmake
enable_testing()
include(GoogleTest)
find_package(GTest 1.10.0)

add_executable(test test.cpp)
target_link_libraries(test GTest::gtest GTest::gtest_main GTest::gmock
                        GTest::gmock_main)
gtest_discover_tests(test)
```