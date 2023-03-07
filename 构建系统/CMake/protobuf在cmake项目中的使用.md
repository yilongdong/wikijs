---
title: protobuf在cmake项目中的使用
description: 
published: true
date: 2023-03-07T09:54:19.059Z
tags: cmake, protobuf
editor: markdown
dateCreated: 2023-03-07T09:54:19.059Z
---

# protobuf在cmake项目中的使用

```cmake
find_package(Protobuf CONFIG REQUIRED)
protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS ${PROJECT_SOURCE_DIR}/src/model/TU.proto)
target_sources(${project_name}-lib PUBLIC ${PROTO_SRCS} ${PROTO_HDRS})
target_include_directories(${project_name}-lib
        PUBLIC src
        PUBLIC ${Protobuf_INCLUDE_DIRS}
        PUBLIC ${CMAKE_CURRENT_BINARY_DIR}
        )
target_link_libraries(${project_name}-lib
        PUBLIC ${Protobuf_LIBRARIES}
        )
```