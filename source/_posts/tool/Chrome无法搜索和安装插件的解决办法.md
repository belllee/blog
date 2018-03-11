---
title: Chrome无法搜索和安装插件的解决办法
date: 2016-07-10 19:39:06
tags: [工具]
---
由于你懂的原因，Google Chrome插件无法搜索和安装。这里提供一个临时解决方案。
1. 打开 http://www.cnplugins.com  或者 http://www.chromeextensions.org 网站（类似网站很多，可以自己搜索）。 搜索您需要的插件，下载到本地。
2. 在chrome地址栏输入 chrome://extensions/ 或者通过菜单打开扩展程序。
3. 将下载的.crx文件拖放到扩展程序页面，然后确认安装即可。

### web前端开发常用的插件推荐
1. 网页调试HTTP测试工具-Postman  http://www.cnplugins.com/devtool/postman/#dlink  
2. web性能分析工具PageSpeed Insights (by Google)  http://www.cnplugins.com/devtool/pagespeed-insights-by-goo/#dlink 

备注：
pagespeed安装后执行分析会出现跑死现象，初步分析是改工具需要连接google获取资源，被墙了：（
可以考虑使用performance-analyser代替，进行性能分析 http://www.cnplugins.com/devtool/performance-analyser/download.html

### 补充postman无法安装的解决办法
1. 使用解压缩软件将crx文件解压缩。将解压后的文件夹里的"_metadata"文件夹改名为"metadata"
2. 打开chrome的扩展程序![图片1.png](https://upload-images.jianshu.io/upload_images/4944427-2a15fe06d12e3d68.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3. 勾选开发者模式，然后点击“加载已解压的扩展程序”，选中.crx文件解压后的目录，即可安装成功![图片2.png](https://upload-images.jianshu.io/upload_images/4944427-c2d1b9399f61ce5c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
4. 使用时在扩展程序中点击“启动”，打开postman后，无需注册也可以使用![图片3.png](https://upload-images.jianshu.io/upload_images/4944427-4482bb1ef4543f90.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
5. 以后每次打开chrome都会出现如下提醒，一定要选择“取消”，否则插件会停用![图片4.png](https://upload-images.jianshu.io/upload_images/4944427-a058ca135e76688d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

