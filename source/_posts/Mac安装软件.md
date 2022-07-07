---
title: Mac安装软件
date: 2022-07-06 13:39:54
tags:
---

**1、**【XXX.app 已损坏，打不开。您应该将它移到废纸篓】解决方法：**
打开终端，输入：

```sh
sudo xattr -r -d com.apple.quarantine /Applications/服务名.app
```

