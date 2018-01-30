---
title: JConsole的远程连接
date: 2018-01-30 14:16:08
tags: [工具,linux，java]
---
### JConsole介绍
JConsole是JDK自带的Java性能分析器，用来监听Java应用程序性能和跟踪代码。默认安装在JDK的bin目录（例如：C:\Program Files\Java\jdk1.8.0_144\bin），直接双击运行即可。  
JConsole可以监听本地的应用，也可以监听远程的应用。在新建连接界面上选择本地应用，或者输入远程连接地址，格式是ip:port，注意这个port是监听端口不是服务端口。
![连接1.jpg](http://upload-images.jianshu.io/upload_images/4944427-73420e670c475117.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
连接完成后进入监听界面，可以查看内存、线程、类、JVM等相关信息。
![监听1.jpg](http://upload-images.jianshu.io/upload_images/4944427-23976ac232a8d753.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 关于远程连接
测试环境部署在RedHat6.5服务器上，一般说明增加如下参数即可允许远程连接。
```java
-Dcom.sun.management.jmxremote.port=8999
-Dcom.sun.management.jmxremote.authenticate=false
-Dcom.sun.management.jmxremote.ssl=false
```
但是实际测试无法连接，经过查询资料，最后配置如下，实现了远程连接。
```java
(java -jar -Dcom.sun.management.jmxremote  -Dcom.sun.management.jmxremote.port=8999 -Dcom.sun.management.jmxremote.rmi.port=9999 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false app-1.0.jar&)
```
同时还需要注意服务器的端口是否被屏蔽，hosts是否配置了实际IP。可以使用hostname -i命令来查询ip是否生效。例如实际ip是10.10.10.101，计算机名是mycomputer。hosts配置如下：
```java
10.10.10.101   mycomputer
10.10.10.101   localhost localhost.localdomain localhost4 localhost4.localdomain4
```
