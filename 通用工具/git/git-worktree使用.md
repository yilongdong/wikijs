---
title: git-worktree使用
description: 
published: true
date: 2023-02-08T00:51:51.833Z
tags: git
editor: markdown
dateCreated: 2023-02-08T00:51:50.784Z
---

使用`git worktree`同时在多个 branch 上工作。

<!-- more -->
<!-- markdownlint-disable MD041 MD002-->

参考[一些文章](https://blog.didispace.com/categories/Git/)

![img](https://pic1.zhimg.com/80/v2-1688c37a2c59e782b3d7e6b4f75ad7f4_1440w.webp)

```shell
git worktree add [-f] [--detach] [--checkout] [--lock [--reason <string>]] [-b <new-branch>] <path> [<commit-ish>]
git worktree list [-v | --porcelain [-z]]
git worktree lock [--reason <string>] <worktree>
git worktree move <worktree> <new-path>
git worktree prune [-n] [-v] [--expire <expire>]
git worktree remove [-f] <worktree>
git worktree repair [<path>...]
git worktree unlock <worktree>
```

使用git worktree可以仅需维护一个 repo，又可以同时在多个 branch 上工作，互不影响。免去分支切换的苦恼。

```shell
 git worktree add -b "feature/feature1" ../feature/feature1
 # 查看链接工作区下.git
 cat .git
# gitdir: /Users/dongyilong/test/git_test/demo-project/.git/worktrees/feature1
```

![image-20221103204829664](https://yilongdong-blog.oss-cn-hangzhou.aliyuncs.com/img/image-20221103204829664.png)

```shell
git worktree list --porcelain
```

![image-20221103204931170](https://yilongdong-blog.oss-cn-hangzhou.aliyuncs.com/img/image-20221103204931170.png)

```shell
git worktree move "feature/feature1" ../feature/feature2
# 不能移动主工作树或者包含子模块的工作树
# 工作树的名字为demo-project， feature/feature2
# git worktree list --porcelain
worktree /Users/dongyilong/test/git_test/demo-project
HEAD e1da10447082e472ad8a2b57808d4ef9cec87823
branch refs/heads/master

worktree /Users/dongyilong/test/git_test/feature/feature2
HEAD e1da10447082e472ad8a2b57808d4ef9cec87823
branch refs/heads/feature/feature1
```

![image-20221103205326277](https://yilongdong-blog.oss-cn-hangzhou.aliyuncs.com/img/image-20221103205326277.png)

```shell
git worktree remove feature/feature2
git worktree remove -f feature/feature2
git worktree prune # 清除$GIT_DIR/worktrees中的工作树信息。
```

