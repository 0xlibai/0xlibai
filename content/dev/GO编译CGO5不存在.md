---
title: "GO编译提示CGO5不存在"
date: 2022-05-30T22:45:04+08:00
lastmod: 2022-05-30T22:45:04+08:00
# type: post
draft: false
slug: "go-cgo-5-not-found"
keywords: ["gcc-5", "go build failed"]
description: "编译GO项目出错C compiler gcc-5 not found"
contentCopyright: false
reward: false
mathjax: false
---

因一个 Go 项目依赖了 go-sqlite3 包，导致在 Windows10 Ubuntu 子系统下编译 Go 项目时报错。

<!--more-->

编译错误信息如下：

```bash
# github.com/mattn/go-sqlite3
cgo: C compiler "gcc-5" not found: exec: "gcc-5": executable file not found in $PATH
```

在 Ubuntu 下尝试使用 brew 安装 gcc-5 但没有对应的版本，也许是 gcc@5 ：

```bash

brew install gcc-5

Running `brew update --preinstall`...
Warning: No available formula with the name "gcc-5". Did you mean gcc@5, gcc, gcc@9, gcc@8, gcc@7 or gcc@6?
==> Searching for similarly named formulae...

These similarly named formulae were found:
gcc@5                          gcc ✔
gcc@9                          gcc@8
gcc@7                          gcc@6
To install one of them, run (for example):
  brew install gcc@5
==> Searching for a previously deleted formula (in the last month)...
Error: No previously deleted formula found.
==> Searching taps on GitHub...
Error: No formulae found in taps.
```

灵光闪现，我直接给已经存在的 gcc 创建了软连接：

```bash
sudo ln -s /usr/bin/gcc /usr/local/bin/gcc-5
```

`go build` 再编译 Go 项目，成功！

谁能告诉我为什么呢？暂且不考虑这神奇的 CGG 问题。
