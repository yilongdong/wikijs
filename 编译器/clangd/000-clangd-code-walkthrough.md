---
title: clangd code walkthrough
description: 
published: true
date: 2023-02-20T08:58:54.545Z
tags: clangd
editor: markdown
dateCreated: 2023-02-20T08:58:54.545Z
---

# clangd code walkthrough
https://clangd.llvm.org/design/code
## Starting up and managing files
[clangd源码导航](https://codebrowser.dev/llvm/clang-tools-extra/clangd/)
### Entry point and JSON-RPC
clangd二进制的`main()`函数入口点在`tool/ClangdMain.cpp文件中`
在这里，主要进行参数解析并且把`ClangdLspServer`hook到它的一些依赖项上。

一个重要的依赖项上`JSONTransport`，它是一个基于stdin/stdout的JSON-RPC协议。LSP就在这个传输抽象之上。
```cpp
int main(int argc, char *argv[]) {
  using namespace clang;
  using namespace clang::clangd;
  // 解析参数略....
  
  // Initialize and run ClangdLSPServer.
  std::unique_ptr<Transport> TransportLayer;

  TransportLayer = newJSONTransport(
      stdin, llvm::outs(),
      InputMirrorStream ? InputMirrorStream.getPointer() : nullptr,
      PrettyPrint, InputStyle);

  ClangdLSPServer LSPServer(*TransportLayer, TFS, Opts);

  int ExitCode = LSPServer.run() ? 0 : static_cast<int>
  return ExitCode;
}
```

### Language Server Protocol
ClangdLSPServer处理LSP协议的详细信息。传入的请求通过查找表路由转到这个类的某个方法，然后通过将它们分派到内部包含的ClangdServer来实现。

传入的json请求被映射到`Protocol.h`中定义的结构体上。在最简单的情况下，`ClangdLSPServer`只是转发给`ClangdServer`的适当方法。

在其他情况下，因为可能lsp与C++api存在一些差距，所以`ClangdLSPServer`会去做一些实际工作。

### ClangdServer and TUScheduler
clangdServer可以当做clangd的C++API。功能倾向于实现为无状态的、同步的函数。`ClangServer`把他们暴露为有状态的异步函数（LSP模型的样子）

`TUScheduler`负责跟踪每个文件的最新版本，构建和缓存AST和作为输入前导，并且提供线程以适当顺序去运行请求。
它还通过`ParsingCallback`将某些事件推送到ClangdServer，以允许发出诊断和索引AST。
Compile commands
Features
Diagnostics
AST-based features
Code completion (and signature help)
Code actions
Feature infrastructure
Parsing and ASTs
Abstractions over clang AST
Index
Dependencies
Clang libraries
Clang-tools libraries
General support libraries
Testing