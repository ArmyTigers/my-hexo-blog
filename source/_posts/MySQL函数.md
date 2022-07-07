---
title: NySQL函数
date: 2022-07-06 13:45:31
tags:
---

### **concat()函数**

1、功能：将多个字符串连接成一个字符串。

2、语法：concat(str1, str2,...)

<!-- more -->

### **concat_ws()函数**

1、功能：和concat()一样，将多个字符串连接成一个字符串，但是可以一次性指定分隔符～（concat_ws就是concat with separator）

2、语法：concat_ws(separator, str1, str2, ...)

### **group_concat()**函数

1、功能：将group by产生的同一个分组中的值连接起来，返回一个字符串结果。

2、语法：group_concat( [distinct] 要连接的字段 [order by 排序字段 asc/desc ] [separator '分隔符'] )