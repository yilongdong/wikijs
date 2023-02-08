---
title: git-commit规范
description: 
published: true
date: 2023-02-08T00:49:13.512Z
tags: git, 规范
editor: markdown
dateCreated: 2023-02-08T00:49:12.303Z
---

点击阅读前文前, 首页能看到的文章的简短描述

<!-- more -->
<!-- markdownlint-disable MD041 MD002-->

http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html

Commit message 都包括三个部分：Header，Body 和 Footer。

```shell
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```

## Header

Header部分只有一行，包括三个字段：`type`（必需）、`scope`（可选）和`subject`（必需）。

**（1）type**

> feat：新功能（feature）
> fix：修补bug
> docs：文档（documentation）
> style： 格式（不影响代码运行的变动）
> refactor：重构（即不是新增功能，也不是修改bug的代码变动）
> test：增加测试
> chore：构建过程或辅助工具的变动

**（2）scope**

`scope`用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

**（3）subject**

`subject`是 commit 目的的简短描述，不超过50个字符。

> - 以动词开头，使用第一人称现在时，比如`change`，而不是`changed`或`changes`
> - 第一个字母小写
> - 结尾不加句号（`.`）

## Body

```shell
More detailed explanatory text, if necessary.  Wrap it to 
about 72 characters or so. 

Further paragraphs come after blank lines.

- Bullet points are okay, too
- Use a hanging indent
```



## Footer

1. 关闭Issue

   ```shell
   # 关闭单个
   Closes #234 
   # 关闭多个
   Closes #123, #245, #992
   ```

   

2. 不兼容变动

感觉基本不用，用到看原文

## Revert

还有一种特殊情况，如果当前 commit 用于撤销以前的 commit，则必须以`revert:`开头，后面跟着被撤销 Commit 的 Header。

```shell
revert: feat(pencil): add 'graphiteWidth' option

This reverts commit 667ecc1654a317a13331b17617d973392f415f02.
```

Body部分的格式是固定的，必须写成`This reverts commit <hash>.`，其中的`hash`是被撤销 commit 的 SHA 标识符。

如果当前 commit 与被撤销的 commit，在同一个发布（release）里面，那么它们都不会出现在 Change log 里面。如果两者在不同的发布，那么当前 commit，会出现在 Change log 的`Reverts`小标题下面。

## 工具

### Commitizen

[Commitizen](https://github.com/commitizen/cz-cli)是一个撰写合格 Commit message 的工具。

```shell
# 安装时遇到报错可以换淘宝源
# npm config set registry https://registry.npm.taobao.org
npm install -g commitizen
```

在项目路径下运行

```shell
commitizen init cz-conventional-changelog --save --save-exact
```

以后，凡是用到`git commit`命令，一律改为使用`git cz`。

如果要编写多行，使用`\n` 换行。回车直接结束描述

![image-20221103214049773](https://yilongdong-blog.oss-cn-hangzhou.aliyuncs.com/img/image-20221103214049773.png)



### standard-changelog

根据commit生成changelog

```shell
npm install -g standard-changelog
cd project_dir
standard-changelog
# npm install -g conventional-changelog
# cd my-project
# conventional-changelog -p angular -i CHANGELOG.md -w
```

![image-20221103215039647](https://yilongdong-blog.oss-cn-hangzhou.aliyuncs.com/img/image-20221103215039647.png)

![image-20221103215056593](https://yilongdong-blog.oss-cn-hangzhou.aliyuncs.com/img/image-20221103215056593.png)

## 修改git commit信息

1、将当前分支无关的工作状态进行暂存

```shell
git stash
```

2、将 `HEAD` 移动到需要修改的 `commit` 上

```shell
git rebase 9633cf0919^ --interactive
```

3、找到需要修改的 `commit` ,将首行的 `pick` 改成 `edit`
4、开始着手解决你的 `bug`
5、 `git add` 将改动文件添加到暂存
6、 `git commit –amend` 追加改动到提交
7、`git rebase –continue` 移动 `HEAD` 回最新的 `commit`
8、恢复之前的工作状态

```shell
git stash pop
```
