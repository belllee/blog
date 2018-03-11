---
title: AngularJS学习--UI-Router
date: 2016-05-09 14:24:08
tags: [JS,AngularJS]
---
# 什么是UI-Router？
AngularJS 是一种富客户端单页面应用框架，所以要在一个页面呈现不同的视图，路由起到了至关重要的作用。 UI-Router是Angular-UI提供的客户端路由框架，它解决了原生的ng-route的不足(视图不能嵌套，同一URL下不支持多个视图)。例如实际的单页面应用中，导航栏用一个视图，内容部分用另一个视图，就是比较常见的。
UI-Router提出了$state的概念。一个$state是一个当前导航和UI的状态，每个$state需要绑定一个URL Pattern。 在控制器和模板中，通过改变$state来进行URL的跳转和路由。

# 如何使用UI-Router
UI-Router模块是一个可选的angularJS模块，如果需要使用，我们要单独引用js。具体应用示例如下：
```
<html>
  <head>
    <script src="lib/angular.js"></script>
    <script src="lib/angular-ui-router.js"></script>
    <script src="helloworld.js"></script>
    <style>.active { color: red; font-weight: bold; }</style>
  </head>
  <body ng-app="helloworld">
    <a ui-sref="hello" ui-sref-active="active">Hello</a>
    <a ui-sref="about" ui-sref-active="active">About</a>
    <ui-view></ui-view>
  </body>
</html>
```
```
var myApp = angular.module('helloworld', ['ui.router']);

myApp.config(function($stateProvider) {
  var helloState = {
    name: 'hello',
    url: '/hello',
    template: '<h3>hello world!</h3>'
  }

  var aboutState = {
    name: 'about',
    url: '/about',
    template: '<h3>Its the UI-Router hello world app!</h3>'
  }

  $stateProvider.state(helloState);
  $stateProvider.state(aboutState);
});
```
## 路由加载的三种方法
```
//1、·调用$state.go方法   
$state.go('detail', {id: 1});   
$state.go('index');

//2、点击包含ui-sref指令的链接
<a ui-sref="contacts">Contacts</a>
<a ui-sref="contacts.detail({contactId: 42})">Contact 42</a>
 
//3、url跳转
<a href="#/index" >index</a>
```

## 视图嵌套
ui-router的视图可以嵌套，视图嵌套通常对应着$state的嵌套。 index.detail是index的子$state，index_detail.html也将作为index.html的子页面。ui-view可以配合$state进行任意层级的嵌套， 即index_detail.html中仍然可以包含一个ui-view，它的$state可能是index.detail.hobbies。

## 视图命名
在ui-router中，一个$state下可以有多个视图，它们有各自的模板和控制器。如上面的示例代码，hello和about就是两个独立的视图，有各自的末班和控制器。


## 参数说明：
- url：默认相对路径（以^开头的是绝对路径）
- views：每个子视图可以包含自己的模板、控制器和预载入数据。 (后2项选填,控制器可以在view中绑定)
- abstract：抽象模板不能被激活 
- template: HTML字符串或者返回HTML字符串的函数
- templateUrl: HTML模板的路径或者返回HTML模板路径的函数
- templateProvider：返回HTML字符串的函数
- controller、controllerProvider：指定任何已经被注册的控制器或者一个作为控制器的函数
- resolve：在路由到达前预载入一系列依赖或者数据，然后注入到控制器中。 
- data：数据不会被注入到控制器中，用途是从父状态传递数据到子状态。 
- onEnter/onExit：进入或者离开当前状态的视图时会调用这两个函数.
- resolve 为控制器提供可选的依赖注入项。是由 key/value 组成的键值对象。
        key – {string}：注入控制器的依赖项名称。
        value - {string|function}：
            string：一个服务的别名
            function：函数的返回值将作为依赖注入项，如果函数是一个耗时的操作，那么控制器必须等待该函数执行完成（be resolved）才会被实例化。
            比如,视图都需要用户登录后才能访问,那么判断是否登录就可以做成一个控制器依赖
            ```
            resolve: {authentication:['topicAuth', '$q', function(topicAuth, $q){
                    return $q.when().then(function(){
                        return topicAuth.authentication();
                    });
                }]}
            ```

## 动态加载模板/内容
在实际使用时往往需要根据参数动态加载不同模板/内容。此时可以通过如下两种方式实现。
```
//1、用参数作为文件名，拼接文件名加载
templateUrl: function ($stateParams){
    return '/topic/detail_' + $stateParams.type + '.html';
}
  
//2、用参数赋值，修改内容
templateProvider: function ($timeout, $stateParams) {
    return $timeout(function () {
        return '<h1>' + $stateParams.topicId + '</h1>'
  }, 100);
}
```
## 路由参数：
ui-router同样支持切换视图时传递参数。默认有如下两种方式传递参数：
1. 通过$stateParams传递。传递的参数是一个object。传递的方法有：
    - ui-sref="index.detail({id:22})“
    - $state.go('index.detail', {id: 22});
2. 通过$location来获取和设置URL。直接在controller里引入$stateParams 和$location即可。

## state 为view controller提供自定义数据。
```
$stateProvider.state('blog.index', {
        templateUrl: ’templates/blog_index.html',
        data: {
            currentPage: 1,
            pageSize: 20
        } 
    })
```
上面 data 对象就是自定义数据,里面定义了页面的当前页和显示内容条数。在视图对应的 controller 中我们就可以通过下面的方法来获取自定义数据.
```
    console.log($state.current.data.currentPage);  // 1
    console.log($state.current.data.pageSize);  // 20
```
## 如何判断选中视图选中
1. $state.includes 
    返回 true / false，查看当前状态是否在某父状态内，比如 $state.includes('contacts')
    ```
    <!-- 包含在 /contacts 状态内部，即其作为 parant state -->
    <li ng-class="{active: $state.includes('contacts')}">
        <a ui-serif="contacts.list">Contacts</a>
    </li>
    ```
2. ui-sref-active 
    查看当前激活状态并设置 Class。例如
    ```
    <li ui-sref-active="active"><a ui-sref="about">About</a></li>
    ```

## 控制器引入的集中方法
```
$stateProvider.state('contacts', {//直接定义
  template:'<h1>test</h1>',
  controller: function($scope){
    $scope.title = 'My Contacts';
  }
}).state('contacts1', {//直接名称引用
  template:'<h1>test</h1>',
  controller: 'ContactsCtrl'
}).state('contacts2', {//直接定义并且定义别名
  template:'<h1>test</h1>',
  controller: function(){
    this.title = 'My Contacts';
  },
  controllerAs: 'contact'
}).state('contacts3', { //控制器名称并且定义别名
  template:'<h1>test</h1>',
  controller: 'ContactsCtrl as contact'
}).state('contacts4', { //根据state动态加载
  template:'<h1>test</h1>',
  controllerProvider: function($stateParams) {
      var ctrlName = $stateParams.type + "Controller";
      return ctrlName;
  }
});
```
注意：如果未定义template，controller将不会被实例化
更多说明：参考 https://github.com/angular-ui/ui-router/wiki