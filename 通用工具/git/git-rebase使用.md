---
title: git-rebase使用
description: 
published: true
date: 2023-02-08T00:51:12.297Z
tags: git
editor: markdown
dateCreated: 2023-02-08T00:51:11.135Z
---

`git rebase`法则：永远不要在公共分支上使用它。

<!-- more -->
<!-- markdownlint-disable MD041 MD002-->



## 合并主干提交到feature

```shell
git checkout feature
git merge master
# git merge feature master
# 这样会在feature创建一个merge commit
```

![将master合并到功能分支中](https://yilongdong-blog.oss-cn-hangzhou.aliyuncs.com/img/02.svg)

```shell
git checkout feature
git rebase master
# git rebase -i master
# 与merge提交不同，rebase通过为原始分支中每个提交创建全新的commits来重写项目历史记录
```

![将功能分支重新映射到主服务器上](https://yilongdong-blog.oss-cn-hangzhou.aliyuncs.com/img/03.svg)



要使用交互式 rebase，需要使用 `git rebase` 和 `-i` 选项：

```css
git checkout feature
git rebase -i master
```

这将打开一个文本编辑器，列出即将移动的所有提交：

```cmake
pick 33d5b7a Message for commit #1
pick 9480b3d Message for commit #2
pick 5c67e61 Message for commit #3
```

此列表准确定义了执行 rebase 后分支的外观。通过更改 `pick` 命令或重新排序条目，你可以使分支的历史记录看起来像你想要的任何内容。例如，如果第二次提交 fix 了第一次提交中的一个小问题，您可以使用以下 `fixup` 命令将它们浓缩为一个提交：

```cmake
pick 33d5b7a Message for commit #1
fixup 9480b3d Message for commit #2
pick 5c67e61 Message for commit #3
```

![使用交互式rebase来压缩提交](https://yilongdong-blog.oss-cn-hangzhou.aliyuncs.com/img/04.svg)

## 本地清理

```shell
git checkout feature
git rebase -i HEAD~3
```

![重新上头~3](https://yilongdong-blog.oss-cn-hangzhou.aliyuncs.com/img/07.svg)

如果要使用此方法重写整个功能，`git merge-base` 命令可用于查找 `feature` 分支的原始 base。以下内容返回原始 base 的提交ID，然后你可以将其传递给 `git rebase`：

```csharp
git merge-base feature master
```

