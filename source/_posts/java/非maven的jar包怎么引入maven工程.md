---
title: 非maven的jar包怎么引入maven工程
date: 2018-01-10 21:41:23
tags: [java]
---
前几天因为业务需要，引入了一个其他部门的jar包。不是Maven工程构建的，也就没有pom文件。这里记录下引入的过程，以备今后参考。  
1. maven仓库中存在的jar包
可以直接在pom文件中添加依赖。但是问题来了，只有jar包，怎么知道groupId和artifactId呢？下面我就用实例告诉大家，怎么引入。比如依赖一个json-lib-2.2.1.jar。只需要在[](http://mvnrepository.com) 网站上查询这个jar包。然后在版本列表中，点击具体需要的版本。
![mvn1-1.jpg](http://upload-images.jianshu.io/upload_images/4944427-0788fe4e3ca5beb2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
进入具体页面后，拷贝需要的依赖配置内容，复制到maven
![mvn1-2.jpg](http://upload-images.jianshu.io/upload_images/4944427-ed06810e63b8067a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
复制maven的配置描述，拷贝到pom文件里即可。
2. maven仓库里不存在的jar包  
可以自己在本地手工添加的方式解决。例如，haha-1.0.jar。我们设置groupId为com.my.test，artifactId为haha。版本为1.0。
a.  maven添加本地jar包
配置好maven环境，有些使用eclipse的童鞋，可能没有安装过maven，会导致无法执行，需要自己配置maven环境。  
执行如下命令：
mvn install:install-file -Dfile=E:\work\haha-1.0.jar -DgroupId=com.my.test -DartifactId=haha -Dversion=1.0 -Dpackaging=jar
b. 手工创建目录
在pom文件里添加配置信息：  
        <dependency>
            <groupId>commons-httpclient</groupId>
            <artifactId>commons-httpclient</artifactId>
            <version>3.1</version>
        </dependency>
创建好目录\.m2\repository\com\my\test\haha\1.0。把haha-1.0.jar文件复制过来，同时新建haha-1.0.jar.pom。参考maven从仓库下载的jar包里相同的文件，复制里面的内容，修改一下groupId，artifactId和version信息即可。执行完这些后，再更新和编辑就可以解决了。