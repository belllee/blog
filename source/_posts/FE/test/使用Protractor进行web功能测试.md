---
title: 使用Protractor进行web功能测试
date: 2018-01-10 21:21:34
tags: [JS,自动化,测试,工具]
---
本文介绍了使用Protractor对AngularJS开发的web应用进行自动化功能测试的方案和详细操作指导。
### 名词解释
Protractor是AngularJS团队发布的一款开源的端到端web测试运行工具。它可以模拟用户的实际交互，帮助验证Angular应用的实际运行状况。Protractor使用Jasmine测试框架来定义测试用例。Protractor为不同的页面交互提供一套健壮的API。相对于其他的端到端的工具，Protractor有着自己的优势，它知道怎么和AngularJS的代码一起运行，特别是应对$digest循环。
### 环境安装
1. 首先必须安装执行环境nodejs
2. 安装浏览器，推荐chrome
3. 安装protractor+webdriver
		npm install protractor -g
		webdriver-manager update 
4. 安装完成后执行protractor --version，检查安装是否正常
5. 在命令行控制台启动Selenium测试服务器。
		webdriver-manager start
		//默认情况下，Selenium测试服务器接入地址为：http://localhost:4444/wd/hub
6. 输出测试报告需要安装相关插件
		//输出html报告
		npm install protractor-jasmine2-html-reporter -g
		//输出junit格式的xml报告
		npm install jasmine-reporters -g
		//由于需要在config文件中加载。一般把这两个插件放在根目录的node_modules 目录下


### 工程配置
配置文件举例如下：
```javascript
var Jasmine2HtmlReporter = require('./node_modules/protractor-jasmine2-html-reporter');
var report = require('./node_modules/jasmine-reporters');
exports.config = {
  // Selenium server 测试服务器接入地址
  SeleniumAddress: 'http://localhost:4444/wd/hub',
  // 测试服务器的配置信息
  multiCapabilities: [{
    browserName: 'firefox'
  },{
    browserName: 'chrome',
    'chromeOptions': {
            'args': ['incognito', 'disable-extensions', 'start-maximized']
        }
  }],
  // 需要运行的测试程序代码文件列表
  suites: {
    scan: 'tc/e2e/scan.js',
    app: 'tc/e2e/app.js',
    hppd: 'tc/e2e/hppd.js'
  },
  // 选择使用 jasmine 作为JavaScript语言的测试框架
  framework: 'jasmine',
  jasmineNodeOpts: {
    showColors: true,
    defaultTimeoutInterval: 30000,
    isVerbose: true,
    includeStackTrace: false
  },
  //输出测试报告
  onPrepare: function(){
    jasmine.getEnv().addReporter(
        new Jasmine2HtmlReporter({
          savePath: 'report/e2e/',
          takeScreenshots: true,  //是否截屏
          takeScreenshotsOnlyOnFailures: true //测试用例执行失败时才截屏
        })
      );
    jasmine.getEnv().addReporter(
      new report.JUnitXmlReporter({
          savePath: 'report/tc/e2e/',
          consolidateAll: false,
          filePrefix:'',
          package:'E2E'
      })
    );
  }
};
```
### 测试用例怎么写
测试用例是基于jasmine框架，关于用例描述和断言比较，请参考上一篇帖子[《基于karma+jasmine的web前端自动化测试》](https://my.oschina.net/u/2492371/blog/796382 "《基于karma+jasmine的web前端自动化测试》")，这里不再赘述了
这里举一个简单的例子。
```javascript
var DOMAIN = '127.0.0.1';
var TARGET_ROOT = 'http://'+DOMAIN;
describe('scan all app', function() {
	beforeAll(function() {
        // 设置cookie
        var cookieObj = {
            "real_name":"放松一下",
            "dept_name":"开发部",
            "avatar":"/images/u15.png",
            "message":"22"
        };
        browser.get(TARGET_ROOT + '/index/about.html');
        browser.manage().deleteAllCookies().then(function () {
               	browser.manage().addCookie("login_user",JSON.stringify(JSON.stringify(cookieObj)), '/', DOMAIN);
        });
	});
    
    it('app.code', function() {
    	browser.get(TARGET_ROOT + '/app/code/');        
    	expect(element.all(by.binding("project['name']")).count()).toBeGreaterThan(1);
    });
   
    it('app.ci', function() {
    	browser.get(TARGET_ROOT + '/app/ci/');        
    	expect(element(by.binding('userInfo.realName')).getText()).toEqual('李忠伟10183089');
    });
});
```
上述事例主要使用了如下接口：
1.  browser.get(url) 访问url指定的web页面
2.  browser.manage().deleteAllCookies() 删除所有cookie
3.  browser.manage().addCookie(key,value) 添加cookie
4.  element.all(locator) 查找locator描述的所有元素
5.  element(locator)  查找locator描述的单个元素

### #浏览器的相关操作
这里举例常用的几个操作，详细的请见参考资料。
```javascript
browser.get(url)   //访问url指定的web页面
browser.close()   //关闭当前窗口
browser.sleep(ms)   //等待，单位毫秒
browser.pause()  //暂停执行，停止在当前页面，主要用于调试
```

### #如何定位元素
上述示例中提到的by.binding，用于定位元素，被称为定位器locator。Protractor中常用的定位器有如下几种：
```javascript
by.id('myElement')   //id为myElement的元素
by.css('[class="element"]')   //根据元素的属性定位元素，此例为样式class为element的元素。同时也支持jquery的selector语法来定位元素，例如（by.css('.element')）。
by.binding('list.title')    //绑定了ng-bind="list.title"的元素
by.repeater('modules.content')    //绑定了ng-repeat="module in modules.content"的元素
by.model('person.name')  //绑定了ng-model="person.nam"的元素
```
更多定位器请见参考资料。

### 定位的元素如何操作
Protractor中使用element(locator)和element.all(locator)来定位元素，前者是定位单个元素，后者是定位所有符合条件的元素。定位到元素后能做哪些操作？常用操作举例如下：
```javascript
element.all(by.binding('list.title')).count()   //返回查找到的元素的个数
element.all(by.css('.element')).get(1)  //返回定查找到的元素中的第二个元素
element(by.css('.myname')).getText()  //返回查找到的元素的text
element(by.id('user_name').sendKeys('user1')    //向查找到的元素输入'user1'
element(by.id('user_desc').sendKeys(protractor.Key.ENTER);    //向查找到的元素输入回车键
element(by.id('user_desc').sendKeys(protractor.Key.TAB);    //向查找到的元素输入TAB键
element(by.id('user_name')).clear();   //清空查找到的元素的内容
element(by.id('submit')).clear();   //点击查找到的元素
```

### 参考资料
[Protractor官网，不过被墙了，你懂的。](http://www.protractortest.org/ "Protractor官网，不过被墙了，你懂的。")
[《Protractor入门》推荐！有可能被墙](http://ramonvictor.github.io/protractor/slides/#/1 "《Protractor入门》推荐！有可能被墙")
[示例比较多的指导书](http://timothymartin.azurewebsites.net/protractor-cheat-sheet/ "示例比较多的指导书")
[《浏览器r相关接口文档》](http://definitelytyped.org/docs/angular-protractor--angular-protractor/classes/protractor.protractor.html "《浏览器相关接口文档》")
[《元素操作的相关接口文档》](http://definitelytyped.org/docs/angular-protractor--angular-protractor/classes/protractor.webelement.html "《元素操作的相关接口文档》")