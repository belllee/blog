---
title: 编码新利器--Visual Studio Code
date: 2018-01-10 19:39:06
tags: [工具]
---
### VSC简介
Microsoft在2015年4月30日Build 开发者大会上正式宣布了 Visual Studio Code项目：一个运行于 Mac OS X、Windows和 Linux 之上的，针对于编写现代 Web 和云应用的跨平台源代码编辑器。  
Visual Studio Code (简称 VS Code / VSC) 是一款免费开源的现代化轻量级代码编辑器，支持几乎所有主流的开发语言的语法高亮、智能代码补全、自定义热键、括号匹配、代码片段、代码对比 Diff、GIT 等特性，支持插件扩展，并针对网页开发和云端应用开发做了优化。软件跨平台支持 Win、Mac 以及 Linux，运行流畅，可谓是微软的良心之作……   

下面简单介绍下个人认为比较酸爽的特性：   
### 集成Git
VSC默认集成了git。打开git项目根目录后，VSC自动会扫描git相关设置，并且默认定时更新代码，扫描状态。如下图，左侧菜单选择git，就能看到变更的文件信息（包括新建，修改，删除）。在信息输入框里输入注释信息，执行Ctrl+Enter，即可完成 git add . 和git commit操作。
![git1png.jpg](http://upload-images.jianshu.io/upload_images/4944427-de4b3dcedbf922f2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  
然后在右上角打开菜单，执行push操作,或者撤销本次提交，以及其他操作。  
双击修改的文件，会自动进行差异对比。如下图   
  
![git3.jpg](http://upload-images.jianshu.io/upload_images/4944427-5e4dd64069840178.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 集成ESLint
ESLint需要自己安装，安装过程如下： 
* 安装ESLint环境。首先安装node.js，然后执行npm install -g eslint安装eslint。
* 安装vsc的eslint插件。点击左侧菜单最后一项，进入插件安装界面。选择 ESLint点击安装，安装后变成设置按钮。  

![plugin.jpg](http://upload-images.jianshu.io/upload_images/4944427-936edcdf8d1fbe11.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
安装完成后，按照提示重启VSC。再次打开后在底部信息框的“问题”标签页里就能看到ESLint检查的结果了。同时在有问题的代码，以及右侧滚动条上也会有错误位置提示。如下图。  

![eslint.jpg](http://upload-images.jianshu.io/upload_images/4944427-226ea44081f94918.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  

注意，代理设置方法如下： 
文件——>首选项——>设置。在右侧配置里增加如下设置
"http.proxy": "http://proxy.my.com.cn:80",
    "https.proxy": "https://proxy.my.com.cn:80",
    "http.proxyStrictSSL": false,


### 支持Node.js调试
打开Node.js项目，左侧菜单选择调试功能。点击顶部的启动调试按钮。第一次调试时，会弹出配置页面。对配置文件launch.json里的参数进行配置。主要是配置下路径和启动文件名，配置完成后再次点击启动即可。如下图

![debug1.jpg](http://upload-images.jianshu.io/upload_images/4944427-98e9adc4ecb8668b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

调试的界面与Chrome dev tool类似，这里就不多介绍了，如下图。

![debug2.jpg](http://upload-images.jianshu.io/upload_images/4944427-881637f5e0a2da45.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 内嵌终端控制器
在底部的信息框里选择“终端”选项卡，就能看到命令行界面了，调入命令执行即可，免去了频繁的切换窗口。如下图：   

![cmd.jpg](http://upload-images.jianshu.io/upload_images/4944427-9c38ee9cab51d5e2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 代码格式化
选中代码，右键，选择格式化
### 代码跳转
右键，选择跳转       
### html、css自动填充
同sublime功能类似，输入标签后按下tab自动补齐。输入字母后，自动关联可能的属性等
### 支持Markdown预览