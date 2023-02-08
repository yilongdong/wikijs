---
title: 开发npm命令行工具
description: 创建于安装npm工具的流程
published: true
date: 2023-02-08T00:31:22.493Z
tags: npm, node
editor: markdown
dateCreated: 2023-02-08T00:31:21.291Z
---

# 开发npm命令行工具

## 创建项目
```shell
mkdir demo
cd demo
npm init
```

## 新建一个可执行文件
在demo文件夹中，新建一个xxx.js文件

```javascript
#!/usr/bin/env node
console.log("Hello World!")
```
内容与普通的 js 文件写法一样，只是前面要加上 `#!/usr/bin/env node`。

这段前缀代码叫 shebang，具体可以参考 [Shebang (Unix) - Wikipedia)](https://en.wikipedia.org/wiki/Shebang_(Unix)).

## 指定执行文件
在package.json中，指定bin字段将xx.js作为我们的执行文件
与包名同名
```json
{
  "name": "demo",
  "bin": "demo.js"
}
```
这样安装的命令名称就是 demo。

自定义命令名称（与包名不同名）
```json
{
  "name": "demo",
  "bin": {
    "abc": "xx.js"
  }
}
```
这样安装的命令名称就是abc

## 调试安装

> npm install
> 我们在写命令行工具的时候，需要指定一个可执行文件。在package.json中，bin字段用来映射命令名和可执行文件。在通过npm install -g全局安装的时候，npm会symlink可执行文件到prefix/bin文件夹。 如果通过npm install本地安装的时候, npm会symlink可执行文件到./node_modules/.bin/文件夹。
> (完整的字段说明在这里：https://docs.npmjs.com/files/package.json)

> npm link
> 在npm包文件夹下执行npm link命令，会创建一个符号链接，链接全局文件夹{prefix}/lib/node_modules/<package>和你执行npm link的包文件夹。本地不再使用了，可以用npm unlink卸载
> 注意：package-name是package.json中的name, 而不是文件夹名。
> 详细的解释在这儿: https://docs.npmjs.com/cli/link

## 发布
不需要发布，所以不写了先，以后用到再补充

## 参考资料

[从 1 到完美，用 node 写一个命令行工具](https://segmentfault.com/a/1190000016555129)
[手把手教你学会写NodeJs的cli工具](http://isweety.me/blog/2018/how-to-write-cli-tool/)
[Node.js CLI Apps Best Practices](https://github.com/lirantal/nodejs-cli-apps-best-practices)