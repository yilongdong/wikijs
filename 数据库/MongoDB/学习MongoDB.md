---
title: 学习MongoDB
description: 
published: true
date: 2023-02-08T00:47:45.396Z
tags: 数据库, mongodb
editor: markdown
dateCreated: 2023-02-08T00:47:44.197Z
---

# MangoDB

## 什么是MongoDB

是一个文档数据库(JSON数据模型)，为web应用提供可扩展的高性能数据存储。介于关系数据库与非关系数据库之间的产品。

半结构化，弱关系。

## MongoDB的应用场景

### 技术优势

MongoDV基于灵活的JSON文档模型，便于敏捷式快速开发。

程序API自然，开发快速。

快速响应业务变化。可动态增加字段。

便于扩展，应用全透明。可以支持TB，PB数量级

### 应用场景

| 特征                               | Yes/No  |
| ---------------------------------- | ------- |
| 不需要复杂/长事务及join支持        | 必须Yes |
| 新应用，需求变化，数据模型无法确定 |         |
| 2000-3000以上的读写QPS             |         |
| TB， PB级别数据存储                |         |
| 快速水平扩展                       |         |
| 数据不丢失                         |         |
| 高可用                             |         |
| 大量的地理位置查询，文本查询       |         |



## MongoDB安装运行

### 安装

[安装数据库服务程序](https://www.mongodb.com/try/download/community)

[安装shell连接工具](https://www.mongodb.com/try/download/shell)

### 启动与关闭

#### 命令行启动

```shell
mkdir -p xx/mongodb/data xx/mongodb/log
bin/mongodb --port=27017 --dbpath=xx/mongodb/data --logpath=xx/mongodb/log/mongodb.log \
--bind_ip=0.0.0.0 --fork
bin/mongosh # 等价于 "mongodb://localhost:27017"
```

> --bind_ip，不设置默认监听localhost
>
> --fork，后台启动
>
> --auth，开启认证模式

#### 配置文件启动

```shell
bin/mongodb -f conf/mongo.conf
bin/mongosh # 等价于 "mongodb://localhost:27017"
```



```yaml
systemLog:
	destination: file
	path: /Users/xx/person/mongodb/log/mongod.log
	logAppend: true
storage:
	dbPath: /Users/xx/person/mongodb/data
	engine: wiredTiger # 支持事务
net:
	bindIp: 0.0.0.0 # 本地和远程都可以访问
	port: 27017 # 27017就是默认端口，不设置也行
processManagement:
	fork: true
```



#### 关闭

在mongosh中

```shell
use admin
db.shutdownServer()
```



## 基础数据库操作

| 命令                            | 说明                           |
| ------------------------------- | ------------------------------ |
| show dbs \| show databases      | 显示数据库列表                 |
| use 数据库名                    | 切换数据库，不存在则创建数据库 |
| db.dropDatabases()              | 删除数据库                     |
| show collections \| show tables | 显示当前数据库集合列表         |
| db.集合名.stats()               | 查看集合详情                   |
| db.集合名.drop()                | 删除集合                       |
| show users                      | 显示当前数据库用户列表         |
| show roles                      | 显示当前数据库角色列表         |
| show profile                    | 显示最近发生的操作             |
| load("xxx.js")                  | 执行一个js脚本文件             |
| exit \| quit()                  | 退出shell                      |
| help                            | 查看支持的命令                 |
| db.help()                       | 查看支持的方法                 |
| db.集合名.help()                | 显示集合帮助信息               |
| db.version()                    | 查看数据库版本                 |



## 安全认证

### 创建管理员账号

```shell
use admin

db.createUser({user:"fox". pwd: "fox", roles:["root"]})

show users

db.dropUser("fox")
```

### 以授权模式启动服务

```shell
./bin/mongod -f ./conf/mongo.conf --auth
```



### 验证是否生效

非认证打开shell

```shell
admin> show dbs
MongoServerError: command listDatabases requires authentication
```

认证连接

```shell
./bin/mongosh --username=fox --password=fox --authenticationDatabase=admin
```



```shell
test> show dbs
admin   132.00 KiB
config  108.00 KiB
local    72.00 Ki
```

### 创建应用数据库用户

```shell
use appdb
db.createUser({user:"appdb",pwd:"fox",roles:["dbOwner"]})
```

登录时使用

```shell
./bin/mongosh --username=appdb --password=fox --authenticationDatabase=appdb
```

## 常用命令

### 创建新数据库与集合

```shell
use myNewDatabase
db.myCollection.insertOne( { x: 1 } );
```

### CRUD操作

[C](https://www.mongodb.com/docs/mongodb-shell/crud/insert/#std-label-mongosh-insert)[R](https://www.mongodb.com/docs/mongodb-shell/crud/read/#std-label-mongosh-read)[U](https://www.mongodb.com/docs/mongodb-shell/crud/update/#std-label-mongosh-update)[D](https://www.mongodb.com/docs/mongodb-shell/crud/delete/#std-label-mongosh-delete)

## GUI工具

Mac arm上没有可用的MongoDB Compass....悲

```shell
Sorry, MongoDB Compass is only supported on 64-bit Intel platforms. If you believe you're seeing this message in error please open a ticket on the SERVER project at https://jira.mongodb.org/
```



### C++ Driver

https://www.mongodb.com/docs/drivers/cxx/
