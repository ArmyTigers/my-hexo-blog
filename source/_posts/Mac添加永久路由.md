---
title: Mac添加永久路由
date: 2022-07-11 09:39:23
tags:
---

# Mac添加永久路由

<!-- more -->
## 查看网卡名称列表
    networksetup -listallnetworkservices

## 指定网卡名称添加路由

```
networksetup -setadditionalroutes "Wi-Fi" 10.123.0.0 255.255.0.0 10.0.0.1
networksetup -setadditionalroutes 'USB 10/100 LAN' 10.123.0.0 255.255.0.0 10.0.0.1
//也可以添加多条路由
networksetup -setadditionalroutes "Wi-Fi" 172.11.0.0 255.255.255.0 172.16.198.1 192.160.0.0 255.255.255.0 172.16.198.1
//注意：上面相当于添加了两条路由
172.11.0.0/24 都从172.16.198.1
192.160.0.0/24 都从172.16.198.1
```
## 查看添加的路由
````shell
networksetup -getadditionalroutes Wi-Fi
````
## 清空路由表
```shell
networksetup -setadditionalroutes Wi-Fi
```




