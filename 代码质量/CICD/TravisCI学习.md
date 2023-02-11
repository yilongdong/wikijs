---
title: TravisCI学习
description: 
published: true
date: 2023-02-11T08:14:07.598Z
tags: travisci, cicd
editor: markdown
dateCreated: 2023-02-09T15:36:17.094Z
---

# TravisCI
https://www.travis-ci.com/
http://www.ruanyifeng.com/blog/2017/12/travis_ci_tutorial.html

Travis CI 提供的是持续集成服务。它绑定 Github 上面的项目，只要有新的代码，就会自动抓取。然后，提供一个运行环境，执行测试，完成构建，还能部署到服务器。

持续集成指的是只要代码有变更，就自动运行构建和测试，反馈运行结果。确保符合预期以后，再将新代码"集成"到主干。

持续集成的好处在于，每次代码的小幅变更，就能看到运行结果，从而不断累积小的变更，而不是在开发周期结束时，一下子合并一大块代码。