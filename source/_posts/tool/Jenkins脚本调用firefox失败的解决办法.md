---
title: Jenkins脚本调用firefox失败的解决办法
date: 2018-01-08 16:54:08
tags: [工具,linux]
---
### 问题描述
jenkins执行的shell脚本里，调用karma进行UT测试，日志显示Cannot start Firefox。karma设置的浏览器是firefox，由于firefox无法启动导致用例无法执行。
### 定位过程
1、开始怀疑是jenkins没有权限，无法调用firefox，给jenkins配置为root权限。可以在脚本中增加 whoami 命令来打印当前用户名称。结果错误依然存在。
2、在在脚本里直接调用firefox，显示错误信息：Error: GDK_BACKEND does not match available displays。经查询，发现是调用firefox时没有传递display ID，导致firefox无法显示界面。
3、原因分析：由于CI服务器上调用jenkins使用的命令是：(java -jar /home/data/jenkins/jenkins.war&) 。此种方式调用jenkins，直接后台进行运行，没有分配显示资源，而且关闭命令窗口，程序依然可以正常执行。但是由于没有display ID，导致firefox无法运行。
### 解决办法
直接在命令窗口中运行jenkins，保持命令窗口。命令为：java -jar /home/data/jenkins/jenkins.war
