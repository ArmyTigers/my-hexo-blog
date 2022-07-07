---
title: Mac终端设置代理
date: 2022-07-06 13:39:00
tags:
---

### 1、这个办法的好处是简单直接，并且影响面很小（只对当前终端有效）。

```sh
export http_proxy=http://proxyAddress:port
```

### 2、把[代理服务器](https://so.csdn.net/so/search?q=代理服务器&spm=1001.2101.3001.7020)地址写入shell配置文件.bashrc或者.zshrc

直接在.bashrc或者.zshrc文件中加入， 这个办法的好处是把代理服务器永久保存了，下次就可以直接用了。

```sh
export http_proxy="http://localhost:port"
export https_proxy="http://localhost:port"
```

<!-- more -->