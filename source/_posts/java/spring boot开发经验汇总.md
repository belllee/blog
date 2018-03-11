---
title: spring boot开发经验汇总
date: 2016-07-09 14:24:08
tags: [spring,java]
---
本文记录了Spring boot开发过程中遇到的一些坑，整理一下，希望能够帮助大家
# spring boot工程在Eclipse中发布
导出jar包方式（以demo工程为例）
1. 工程右键，选择export-》java-》Runnbale JAR file![](https://upload-images.jianshu.io/upload_images/4944427-43adf772b1f7a9b5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2. 选择launch configuration,设置输出目标文件，勾选依赖库导入到单独目录（demo_lib）
3. 将工程属性文件，例如application.properties拷贝到jar文件所在目录。
完成的发布包应该包括：demo.jar，demo_lib，application.properties等文件
执行 java -jar demo.jar即可启动整个项目

# 已有数据表如何导入入到项目的Entity
使用jpa工具导入
在 eclipse matketpalce里搜索jpa，选择 JPA Diagram Editor进行安装
在工程的属性里，选择 project facets 再选中JPA
工程右键，选择JPA Tools->Generate Entities from Tables。然后配置数据库连接字，选择数据表，生成entity

# 上传文件大小最大值设置
Spring boot默认最大上传文件是10Mb，如果要改变这个阈值需要自己设置。不同Spring boot版本存在差别。举例如下：
```
//例如在application.properties
//Spring Boot 1.3.x或者之前
multipart.maxFileSize=500Mb
multipart.maxRequestSize=1000Mb
//Spring Boot 1.4.x或者之后
spring.http.multipart.maxFileSize=500Mb
spring.http.multipart.maxRequestSize=1000Mb
//Spring Boot 2.0.x或者之后
spring.servlet.multipart.maxFileSize=500Mb
spring.servlet.multipart.maxRequestSize=1000Mb
```
注意：如果有使用zuul路由服务，还需要设置zuul的相关参数。

# EntityNotFoundException
使用ManyToOne进行表关联且懒加载时，如果被关联的记录不存在时，序列化时报如下异常：
```
//关联定义如下
@JoinColumn(name="topic_id")
@JsonView(JsonViews.Public.class)
@ManyToOne(fetch=FetchType.LAZY)
public Topic getTopic() {
    return topic;
}
```
异常信息如下：
Could not write content: Unable to find com.demo.entity.topic.Topic with id 14363
//跟踪代码，能发现
javax.persistence.EntityNotFoundException: Unable to find xxx
如果关联记录不存在，还需要返回结果，增加@NotFound(action= NotFoundAction.IGNORE)，解决方案：
```
@JoinColumn(name="topic_id")
@ManyToOne(fetch=FetchType.LAZY)
@NotFound(action= NotFoundAction.IGNORE)
public Topic getTopic() {
    return topic;
}
```
# 无法下载maven依赖库
现象
下载依赖库失败。 Failure to transfer org.apache.maven.plugins:maven-compiler-plugin:pom:2.0.2 from http://repo1.maven.org/maven2 was cached in the local repository

解决办法
在下面两个文件中的mirrors 组中增加自己公司里的私有仓库
The Maven install: $M2_HOME/conf/settings.xml
A user’s install: ${user.home}/.m2/settings.xml
```
<mirrors>
<mirror>
 <id>nexus-demo</id>
 <mirrorOf>*</mirrorOf>
 <name>Nexus demo</name>
 <url>http://maven.demo.com.cn/content/groups/public</url>
</mirror>
</mirrors>
```
然后重新update，注意勾上强制更新。
STS里如果更新失败，可以尝试直接命令行 mvn clean & mvn install

# 其他异常
## Not an managed type: class com.demo.entity.Approve
class Approve定义没有添加注解@Entity

## No qualifying bean of type [com.demo.service.ApplyService] found for dependency
没有在config/JPAConfig.java里正确配置package信息。或者是没有给加上@Service注解。