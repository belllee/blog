---
title: 基于karma+jasmine的web前端自动化测试
date: 2018-01-10 21:24:35
tags: [JS,自动化,测试,工具]
---
本文介绍了基于karma+jasmine的web前端自动化测试的方案和详细操作指导。
### 名词解释
1. Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效。Node.js 的包管理器 npm，是全球最大的开源库生态系统。
2. Karma是一个基于Node.js的JavaScript测试执行过程管理工具（Test Runner）。该工具可用于测试所有主流Web浏览器，也可集成到CI（Continuous integration）工具，也可和其他代码编辑器一起使用。这个测试工具的一个强大特性就是，它可以监控(Watch)文件的变化，然后自行执行，通过console.log显示测试结果。
3. Jasmine 是一个简易的JS单元测试框架。Jasmine 不依赖于任何浏览器、DOM、或者是任何 JavaScript 而存在。它适用于所有网站、Node.js 项目，或者是任何能够在 JavaScript 上面运行的程序。

### 环境安装
1. 首先必须安装执行环境nodejs
2. 安装浏览器，推荐chrome（用于运行监听程序，监听js文件变化，自动触发测试执行）
3. 安装karma+jasmine
		npm install karma -g
		npm install karma-jasmine -g
		npm install karma-chrome-launcher -g 
		npm install karma-cli -g 
		npm install karma-coverage -g 
		npm install karma-html-reporter -g 
4. 安装完成后执行karma -v，检查安装是否正常

### 工程配置
1. 可以使用karma init，自动生成配置文件，完成部分参数的设置，然后再手动修改。
2. 当然最快的配置方法，复制下面的配置
```javascript
		// Karma configuration
		// Generated on Tue Nov 01 2016 14:17:00 GMT+0800 (中国标准时间)

		module.exports = function(config) {
		  config.set({
		  // base path that will be used to resolve all patterns (eg. files, exclude)
			basePath: '',

        // frameworks to use
        // available frameworks: https://npmjs.org/browse/keyword/karma-adapter
        frameworks: ['jasmine'],
    
        // list of files / patterns to load in the browser
        //需要加载入浏览器的js文件，包括基础的类库，被测试js文件和测试用例js文件
        //如果需要测试angular代码，比如引入angular-mock.js，给angular代码进行mock。
        //注意angular-mock的版本一定要和angular版本一致。可在cdn网站对应的angular版本列表中寻找
        files: [
            '../webapp/vender/jquery/jquery-1.10.2.min.js',
            '../webapp/vender/angular/angular.min.js',
            '../webapp/vender/angular/angular-ui-router.min.js',
            'lib/angular-mocks.js',
            '../webapp/common/*.js',
            '../webapp/commont/template/*.html',
            'tc/ut/**/*.js'
        ],
    
        // list of files to exclude
        exclude: [
          //'../webapp/vender/**/*.js'
        ],
    
        // test results reporter to use
        // possible values: 'dots', 'progress'
        // available reporters: https://npmjs.org/browse/keyword/karma-reporter
        //这里定义输出的报告
        //html对应karma-html-reporter组件，输出测试用例执行报告
        //coverage对应karma-coverage组件，输出测试用例执行报告
        reporters: ['progress', 'html', 'junit', 'coverage'],
        junitReporter: {  
               // will be resolved to basePath (in the same way as files/exclude patterns)  
              outputFile: 'report/ut/test-results.xml',
              suite: 'UT',
              useBrowserName: false 
        },  
        htmlReporter: {
          outputDir: 'report/ut',
          reportName: 'result' //outputDir+reportName组成完整的输出报告格式，如没有定义，会自动生成浏览器+OS信息的文件夹，不方便读取报告
        },
        //定义需要统计覆盖率的文件
        preprocessors: {
            '../webapp/common/*.js':'coverage', 
            '../webapp/common/template/*.html': 'ng-html2js'
        },
        coverageReporter: {  
            type: 'html', //将覆盖率报告类型type设置为cobertura 或者 html
            subdir:'coverage', //dir+subdir组成完整的输出报告格式，如没有定义，会自动生成浏览器+OS信息的文件夹，不方便读取报告
            dir: 'report/ut/'  //代码覆盖率报告生成地址
        },
    
        // web server port
        port: 9876,
    
        // enable / disable colors in the output (reporters and logs)
        colors: true,
    
        // level of logging
        // possible values: config.LOG_DISABLE || config.LOG_ERROR || config.LOG_WARN || config.LOG_INFO || config.LOG_DEBUG
        logLevel: config.LOG_INFO,
    
        // enable / disable watching file and executing tests whenever any file changes
        //karma自动自动监视被测试文件和测试用用例文件，如有修改，自动重新执行测试
        autoWatch: true,
        // Continuous Integration mode
        // if true, Karma captures browsers, runs the tests and exits
        //上一个参数为true，本参数为false，，则自动监视才生效。否则执行完测试用例后自动退出
        singleRun: true,
    
        // start these browsers
        // available browser launchers: https://npmjs.org/browse/keyword/karma-launcher
        //用来执行自动监听的浏览器，推荐chrome
        browsers: ['Chrome'],
    
        // Concurrency level
        // how many browser should be started simultaneous
        concurrency: Infinity，
       //自动将模板文件路径转换页面引入路径，以便注入用例中
        ngHtml2JsPreprocessor: {
             cacheIdFromPath: function(filepath) {
                 var cacheId = filepath.substr(filepath.lastIndexOf('/webapp/')+7);
                 // console.log(cacheId);
                 return cacheId;
               },
           moduleName: 'template'
        }
		})
		}
```    
3. 保存配置文件到测试目录

### 测试用例编写
#### 1、用例怎么写
```javascript
describe("A suite of Common/common.js", function() {
	
	beforeAll(function(){
		console.log('beforeAll');
	});
   describe("extends of String", function() {	
	   var expected;
		beforeEach(function(){
			expected = 'abcd';
		});
		it("trim",function(){
			expect(expected).toEqual((" abcd ").trim());
		});
		it("ltrim",function(){
			expect(expected).toEqual((" abcd").ltrim());
		});
		it("rtrim",function(){
			expect(expected).toEqual(("abcd ").rtrim());
		});
	});
});
```
上述例子中，
a. describe相当于一个测试套，可以嵌套。
b. it('tc name',function(){})是一个测试用例。
c. beforeAll和beforeEach是预置条件，前者一个测试套执行一次，后者每个测试用例执行一次。
d. 当然还会有afterAll和afterEach
e. expect是断言

#### 2、 断言都有那些比较
```javascript
Matcher实现了断言的比较操作，将Expectation传入的实际值和Matcher传入的期望值比较。任何Matcher都能通过在expect调用Matcher前加上not来实现一个否定的断言（expect(a).not().toBe(false);）。
常用的Matchers有：
toBe()：相当于= =比较。
toNotBe():相当于! =比较。
toBeDefined()：检查变量或属性是否已声明且赋值。
toBeUndefined()
toBeNull()：是否是null。
toBeTruthy()：如果转换为布尔值，是否为true。
toBeFalsy()
toBeLessThan()：数值比较，小于。
toBeGreaterThan()：数值比较，大于。
toEqual()：相当于==，注意与toBe()的区别。一个新建的Object不是（not to be）另一个新建的Object，但是它们是相等（to equal）的。
expect({}).not().toBe({});
expect({}).toEqual({});
toNotEqual()
toContain()：数组中是否包含元素（值）。只能用于数组，不能用于对象。
toBeCloseTo()：数值比较时定义精度，先四舍五入后再比较。
toHaveBeenCalled()
toHaveBeenCalledWith()
toMatch()：按正则表达式匹配。
toNotMatch()
toThrow()：检验一个函数是否会抛出一个错误
```

#### 3、 angular代码怎么写
先看例子

```javascript
describe('Apply MainCtrl', function() {
  var $scope,
      $controller,
      $httpBackend;
     var MainCtrl;

  beforeEach(module('applyApp'));

  beforeEach(inject(function(_$controller_, $rootScope,  _$httpBackend_) {
    $scope = $rootScope.$new();
    $httpBackend = _$httpBackend_;
    $controller = _$controller_;
    $httpBackend.when('POST', /\/api\/wxcop\/common\/record.*/).respond({});      
  }));

    it('Check $scope assignments.', function() {
      MainCtrl = $controller('MainController', {
            $scope: $scope
        });      
      $httpBackend.flush();
      $scope.gotoApplyHome();
      $scope.judgeLogin();
      expect($scope.typeSelect).toEqual(["单行文本","多行文本","单选","多选"]);
      expect($scope.getItemItems("1,2，3")).toEqual(["1","2","3"]);
    });
});

```
注意，要测试angular必须引入angular-mock。
说明
1. beforeEach(module('applyApp'));  引入module 'applyApp'
2. beforeEach(inject(function(_$controller_, $rootScope,  _$httpBackend_)  依赖注入和http测试打桩
3.  $controller('MainController',  )初始化controller
4. 直接调用scope，然后执行断言

#### 5、 angular的相关特性如何测试
##### 1、测试函数
a.  被测试代码
```javascript
$scope.functionTriger = false;
$scope.doTest = function(){
	$scope.functionTriger = true;
}
```
b. 测试用例
```javascript
it('function', function() {    
    expect($scope.functionTriger).toBeFalsy();
    $scope.doTest();
    expect($scope.functionTriger).toBeTruthy();
});
```
##### 2、测试监听
a.  被测试代码
```javascript
$scope.watchVar = false;
$scope.watchedTrigeIndex = 0;
$scope.$watch('watchVar', function() {
    $scope.watchedTrigeIndex++;
});
```
b. 测试用例
```javascript
it('watch', function() {    
    expect($scope.watchedTrigeIndex).toBe(0);
    $scope.watchVar = true;
    $scope.$digest();
    expect($scope.watchedTrigeIndex).toBe(1);
    $scope.watchVar = false;
    $scope.$digest();
    expect($scope.watchedTrigeIndex).toBe(2);
});
```

##### 3、测试广播
a.  被测试代码
```javascript
$scope.isHaveTriger = false;
$scope.$on('ngRepeatFinished', function(){
    $scope.isHaveTriger = true;
});
```
b. 测试用例
```javascript
  it('broadcast', function() {    
    expect($scope.isHaveTriger).toBeFalsy();
    $scope.$broadcast('ngRepeatFinished');
    expect($scope.isHaveTriger).toBeTruthy();
  });
```
##### 4、测试路由切换
a.  被测试代码
```javascript
.config(['$stateProvider', '$urlRouterProvider', function($stateProvider, $urlRouterProvider){
    $urlRouterProvider.otherwise("/detail");
    $stateProvider.state('detail', {
        url:'/detail',
        template:'<p></p>',
        controller:'MainCtrl'
    });
}])
```
b. 测试用例
```javascript
it('route', function() {  
    inject(function (_$injector_) {
      $state = _$injector_.get('$state');
    });  
    var curState = $state.get('detail');
    expect(curState.name).toEqual('detail');
    expect(curState.url).toEqual('/detail');
    expect(curState.controller).toEqual('MainCtrl');
    expect(curState.template).toEqual('<p></p>');
  });
```

##### 5、测试过滤器
a.  被测试代码
```javascript
.filter('myFilter', function(){
    return function(data) {    
        return data + 'lzw';
    }
})
```
b. 测试用例
```javascript
  it('filter', function() {
    inject(function (_$injector_) {
      $filter = _$injector_.get('$filter');
    });
      
```

##### 6、测试service
a.  被测试代码
```javascript
.service('foo', function() {
    var thisIsPrivate = "Private";
    function getPrivate() {
        return thisIsPrivate;
    }
    return {
        variable: "This is public",
        getPrivate: getPrivate
    };
})
```
b. 测试用例
```javascript
it('service',function(){
    var foo;
    inject(function(_foo_) {
        foo = _foo_;
    });
    expect(foo.variable).toBe('This is public');
    expect(foo.getPrivate()).toBe('Private');
  });
```

##### 7、测试指令
a.  被测试代码
```javascript
.directive('myDirective', function() {
    return {
        restrict: 'A',
        replace: true,
        template: '<p>11</p>',
        link: function(scope) {}
    };
})
.directive('dirButton', function() {
    return {
        template: '<button>Increment value!</button>',
        link: function (scope, elem) {
            elem.find('button').on('click', function(){
                scope.value++;
            });
        }
    };
})
.directive('dirScope', function() {
    return {
        scope:{
            config: '=',
            notify: '@',
            onChange:'&'
        },
        link: function(scope){
        }
    };
})
```
b. 测试用例
```javascript
it('directive', function(){    
    var link = $compile('<p my-directive></p>');
    var element = link($scope);
    expect($(element).html()).toBe('11');
  });

  it('button directive', function () {
    var directiveElem = $compile('<button dir-button></button>')($scope);
    $scope.value=10;
    var button = directiveElem.find('button');
    button.triggerHandler('click');
    $scope.$digest();
    expect($scope.value).toEqual(11);
  });

  it('scope directive',function(){
    $scope.config = {
      prop: 'value'
    };
    $scope.notify = true;
    $scope.onChange = jasmine.createSpy('onChange');
    var directiveElem = $compile(angular.element('<p dir-scope config="config" notify="notify" on-change="onChange()"></p>'))($scope);
    $scope.$digest();
    var isolatedScope = directiveElem.isolateScope();

    //test =    
    isolatedScope.config.prop = "value2";
    expect($scope.config.prop).toEqual('value2');

    //test @
    isolatedScope.notify = false;
    expect($scope.notify).toEqual(true);

    //test &
    expect(typeof(isolatedScope.onChange)).toEqual('function');
    isolatedScope.onChange();
    expect($scope.onChange).toHaveBeenCalled();
	
   //调用指令的父controller。
   directiveElem.scope().doFunction();
  });
  
```
### 关于mock
#### $httpBackend
$httpBackend对于代码中的http请求进行mock。常用方法：
```javascript
$httpBackend.when(method, url, [data], [headers]);
$httpBackend.expect(method, url, [data], [headers]);
```
when和expect都有对应的快捷方法:
```javascript
whenGET(url, [headers]);
whenHEAD(url, [headers]);
whenDELETE(url, [headers]);
whenPOST(url, [data], [headers]);
whenPUT(url, [data], [headers]);
whenJSONP(url);
expectGET(url, [headers]);
expectHEAD(url, [headers]);
expectDELETE(url, [headers]);
expectPOST(url, [data], [headers]);
expectPUT(url, [data], [headers]);
expectPATCH(url, [data], [headers]);
expectJSONP(url);
```
url支持正则，比如：
```javascript
$httpBackend.when('POST', /\/api\/wxcop\/common\/record.*/).respond({});
```
注意：
$httpBackend.when与$httpBackend.expect的区别在于：$httpBackend.expect的伪后台只能被调用一次(调用一次后会被清除)，第二次调用就会报错，而且$httpBackend.resetExpectations可以移除所有的expect而对when没有影响。

### 常见异常处理
##### Argument 'MainCtrl' is not a function, got undefined
无法找到MainCtrl。可能原因：controller定义错误，app注入失败。
##### Disconnected, because no message in 10000 ms.
ajax请求超时。原因：$httpBackend.flush();要放在ajax发起请求后执行。
##### 指令采用templateUrl方式加载模板失败
可采用karma-ng-html2js-preprocessor插件自动注入。也可以采用$templateCache注入。注意，这两种方式都不支持模糊匹配


### 参考资料
[《AngularJS Testing Tips: Testing Directives》](https://www.sitepoint.com/angular-testing-tips-testing-directives/ "《AngularJS Testing Tips: Testing Directives》")
[《Unit Testing in AngularJS: Services, Controllers & Providers》](https://www.sitepoint.com/unit-testing-angularjs-services-controllers-providers/)
[《Unit Testing Services in AngularJS for Fun and for Profit》](https://nathanleclaire.com/blog/2014/04/12/unit-testing-services-in-angularjs-for-fun-and-for-profit/)