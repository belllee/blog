---
title: Grunt--web前端构建工具
date: 2016-09-10 19:39:06
tags: [工具]
---
#简介
Grunt是一款使用javascript开发的开源的web前端构建工具，有着丰富的给类工具插件。Grunt及其插件运行在node.js环境上。可以完成诸如：html/css/js/image文件的压缩和合并，html/css/js文件的静态检查，js的自动化测试，引入css/js文件的版本号自动更新，文件的删除和复制等等，web前端开发所需要的各类工作。

Grunt官网：[http://gruntjs.com/](http://gruntjs.com/ "http://gruntjs.com/")

非官方中文网站：[http://www.gruntjs.net/](http://www.gruntjs.net/ "http://www.gruntjs.net/")
#Grunt的安装
Grunt在node.js环境上执行，使用前必须先安装node.js和npm。
##node.js的安装
直接到官网下载安装包，执行安装即可。下载地址：[https://nodejs.org/en/](https://nodejs.org/en/)

安装了node.js之后，打开命令行控制台，输入“node -v”来查看node。js的版本，已验证node.js是否安装成功。npm集成在node.js的安装包，安装完node.js即可。输入“npm -v”，查看npm版本。

##Grunt的安装
由于npm经常被墙，导致无法安装成功。幸好我们有了中兴自己的镜像。
执行“npm set registry http://mirrors.zte.com.cn/npm” 配置好镜像后，就能顺利完成安装了。执行“npm install -g grunt-cli” 安装Grunt，注意安装的是grunt-cli，-g表示全局安装，所有目录均可使用

安装完成后，执行“grunt -v”。验证安装是否成功。

#Grunt插件的安装
安装Grunt插件的时候可以采用两种方式，一种是单个插件安装（常用于新搭建项目的场景）；一种是插件批量安装（常用于下载项目文件后搭建开发环境）。具体安装方法如下：
1. 安装单个插件
	首先在命令行控制台里，切换到项目根目录。例如安装jshint插件，执行“npm install grunt-contrib-jshint --save-dev”命令即可。

2. 批量安装
	首先新建package.json文件。将需要安装的插件名称和版本填写好（如果是配置库上下载，已有配置文件，此步骤跳过）。详细内容参考下一小节。
	命令行控制台切换到项目根目录。执行“npm install --save-dev”命令即可。执行成功后，根目录下会出现node_modules目录，下面会出现已安装的所有插件。
##package.json配置文件
配置具体示例如下：
```javascript
{
  "name": "wxcop",
  "version": "0.1.0",
  "devDependencies": {
    "grunt": "^0.4.5",
    "grunt-contrib-clean": "^0.6.0",
    "grunt-contrib-copy": "^0.8.2",
    "grunt-contrib-cssmin": "^0.14.0",
    "grunt-contrib-jshint": "^0.11.3",
    "grunt-contrib-uglify": "^0.9.2"
  }
}
```
示例中name为当前工程名称，version工程版本信息，devDependencies为依赖项（即Grunt和相关插件）。注意，这里的版本号前面有一个^符号，表示该插件可以被Grunt自动更新到最新的子版本。
关于配置文件的说明，详见：[https://docs.npmjs.com/files/package.json](https://docs.npmjs.com/files/package.json)
#配置构建任务
在项目根目录下新建构建任务描述文件Gruntfile.js。具体示例如下：
```javascript
module.exports = function(grunt) {  
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),   //加载配置文件 
    uglify: {
      options: { },
      buildall: {//按原文件结构压缩js文件夹内所有js文件
          options: {
            preserveComments:false,
            report: "min"//输出压缩率，可选的值有 false(不输出信息)，gzip
          },
          files: [{
              compress:false,
              expand:true,
              cwd:'js',//js目录下
              src:'**/*.js',//所有js文件
              dest: 'dist/js'//输出到此目录下
          }]
      },
      ueditor: {//单独压缩ueditor.all.js文件
          options: {
            preserveComments:false,
            report: "min"
          },
          files: [{
              compress:false,
              src:'vender/ueditor/ueditor.all.js',
              dest: 'vender/ueditor/ueditor.all.min.js'
          }]
      }
    },
    cssmin: {
      compress: {//按原文件结构压缩css文件夹内所有css文件
        files: [{
               expand: true,
               cwd: 'css/',
               src: '**/*.css',
               dest: 'dist/css'
         }]
      },
      adminlte: {//单独压缩AdminLTE.css文件
          files: [{
              src:'vender/AdminLTE/AdminLTE.css',
              dest: 'vender/AdminLTE/AdminLTE.min.css'
          }]
      }
    }  
  });
  // 加载包含 "uglify" 任务的插件。
  grunt.loadNpmTasks('grunt-contrib-uglify');
  grunt.loadNpmTasks('grunt-contrib-cssmin');
  // 默认被执行的任务列表。
  grunt.registerTask('default', ['uglify','cssmin:compress','cssmin:adminlte']);
};
```
上述代码实现了js目录下所有js文件的压缩，css目录下的所有css文件的压缩。文件主题是module.exports = function(grunt) { };函数内部主要有三部分：初始化参数，加载插件，注册构建任务。
1. 初始化参数
	pkg属性，指定具体加载的配置文件，默认的必选项。uglify和cssmin是自己定义的任务，名称也是自定义的。
2. 加载插件
	加载指定的插件。
3. 注册构建任务
	default是任务名称，[]中是具体构建项，按照先后顺序执行。可以是一个任务集合'uglify',也可以是一个具体的任务'cssmin:compress'。
#执行构建
在命令行控制台里，切换到项目根目录。执行grunt，即可执行default任务。执行“grunt cssmin:adminlte”，即可执行具体的一个任务。执行“grunt uglify”即可执行一个任务集合。

这里只是对grunt的一个概括介绍，配置的详细参数，具体插件的配置，后续在其他帖子里描述。