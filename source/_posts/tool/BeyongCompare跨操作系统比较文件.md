---
title: BeyongCompare跨操作系统比较文件
date: 2016-07-10 19:39:06
tags: [工具]
---
# 问题现象
一份代码在windows上，一份在linux上。直接用BC比较，js和css文件，结果文件全部红色，被判为有差异。实际文件没有修改，必须挨个文件打开，才能知道是否有变化。其他文件无此问题。如果两份代码全部在windows上也没有此问题。
# 问题原因
windows和linux上对换行符的定义是不一样的，所以导致BC因为有变化。
# 解决办法
在BC里面选择基于规则比较。
![](https://static.oschina.net/uploads/img/201803/09212530_pb36.jpg)

打开一个JS文件选择规则，设置“比较行结尾”
![](https://static.oschina.net/uploads/img/201803/09212726_UN7r.jpg)