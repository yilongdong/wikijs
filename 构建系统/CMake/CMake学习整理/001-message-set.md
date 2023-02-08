---
title: 001-message-set
description: 注释,message, set, include
published: true
date: 2023-02-07T01:46:19.534Z
tags: cmake
editor: markdown
dateCreated: 2023-02-07T01:35:26.901Z
---

# 注释,message, set, include

## 注释



1. 行注释

```cmake
# 注释
```

2. 括号注释(3.0之后)

```cmake
#[[第一行注释
第二行注释]]
message("arg1\n" #[[注释内容]] "arg2")
```



## message

### message日志级别

```cmake
message(arg1 arg2 arg3)
message([<mode>] "message text" ...)

# mode
#[[
FATAL_ERROR 停止cmake运行和生成
SEND_ERROR cmake继续运行，生成跳过
WARNING 打印到stderr 会打印路径和行数
(none) or NOTICE 打印到stderr
STATUS 项目用户可能感兴趣的信息，会加前缀--
VERVOSE 针对项目用户的详细信息，会加前缀--，默认不显示
DEBUG 开发人员使用的信息
TRACE 非常低级实现细节的消息
]]


cmake -S . -B build --log-level=VERBOSE
```

### message report checks插座库日志

```cmake
message(<checkState> "msg" ...)
# 可以嵌套
# checkState
#[[
CHECK_START
CHECK_PASS
CHECK_FAIL
]]


```



```cmake

message(CHECK_START "find xxx")
# 增加缩进
set(CMAKE_MESSAGE_INDENT "--")

# 嵌套 1
message(CHECK_START "find yyy")
message(CHECK_PASS "found")
# 嵌套 2
message(CHECK_START "find zzz")
message(CHECK_FAIL "not found")

# 取消缩进
set(CMAKE_MESSAGE_INDENT "")
message(CHECK_FAIL "not found")
```

## 变量

变量未设置返回空字符串, `${name}`

变量引用可以嵌套，并由内而外求值`${prefix${name}}`

变量名大小写敏感

`set`(), `unset()`

```cmake
set(VAR1 "VAR1_value")
message("\${VAR1}=${VAR1}")
message("\${VAR1}=${VAR1}")
```

### cmake内建变量

#### 提供信息的变量

1. `PROJECT_NAME` 项目名

#### 改变行为的变量

1. `BUILD_SHARED_LIBS`

#### 描述系统的变量



#### 控制构建过程的变量

`CMAKE_COLOR_MAKEFILE`,默认`ON`



### cmake传递变量给C++

1. CLI

   `cmake .. -Dxx=yy -Dzz`

2. cmake

   `add_definitions(-Dxx=yy -Dzz)`

   zz默认值为1



## include make模块

```cmake
include("cmake/test.cmake")

include("cmake/xx.cmake" OPTIONAL) # 可选，文件不存在不报错

include("cmake/xx.cmake" OPTIONAL RESULT_VARIABLE ret) # 获得是否找到的返回值
message("RESULT_VARIABLE ret = ${ret}") # ret = NOFOUND

include("cmake/test.cmake" OPTIONAL RESULT_VARIABLE ret) # 获得是否找到的返回值
message("RESULT_VARIABLE ret = ${ret}") #ret = 导入的cmake文件路径


```

