---
title: AngularJS学习--Services
date: 2016-04-02 12:24:08
tags: [JS,AngularJS]
---
# Services是什么？
AngularJS里Services主要负责提供一个接口把特定功能的方法放在一起。类似封装好的类库供其他controller和directive调用。同时可以在应用中的不同作用域之间共享数据。Services作为单例对象在需要使用的时候被创建，只有在应用生命周期结束的时候（关闭浏览器）才会被清除。而controllers在不需要的时候就会被销毁。Service都是单例的，就是说在一个应用中，每一个Serice对象只会被实例化一次（用$injector服务)。

# Services类型
AngularJS带来了很多类型的Services定义方法。每个方法都会它自己不同的使用场景。下面我们会分别介绍。
## 1、Constant
```
app.constant('myConfig', {
    isLoad : true,
    des : "default config"
});
```
Constant就是常量，赋值后不能被修改，常用于对directive等做配置信息。所以当你想创建一个directive，并且你希望能够做一些配置信息，同时给些默认的配置，constant是个不错的的选择。常量可以是基础类型或object对象。
## 2、Value
```
app.value('myConfig', {
    isLoad : true,
    des : "default config"
});
```
Value和Constant很相似，唯一区别是其在赋值后还可以被改变。它也被常用于directive配置信息。也可以理解为全局变量。
## 3、Factory
```
app.factory('myFactor', function() {
	var name = "myFactor";
	function getName() {
		return name;
	}
	return {
		des: "This is factory service",
		getName: getName
	};
});
```
Factory是我们最常用的service,会返回一个object对象。可以理解为工厂模式。Factory创建可以有多种方式，下面的代码就是另外一种创建方式。
```
app.factory('myFactor', function() {
	var myInstance= {
        var des = "This is factory service";
	    var name = "myFactor";
	    function getName() {
            return name;
        }
    };
	return myInstance;
});
```
## 4、Service
```
app.service('myService', function() {
    var name = "myService";
    this.des = "This is factory service";
    this.getName = function() {
	return name;
    };
});
```
service和factory工作原理一样，只是service接收的是一个构造函数，当第一次使用service的时候，angular会new MyService() 来初始化这个对象。后续使用的时候返回的都是同一个对象。下面是factory等价的写法：
```
function MyService() {
	var name = "myService";
	this.des = "This is service";
	this.getName = function() {
		return this.name;
	};
}
app.factory('myService', function() {
	return new MyService();
});
```
## 5、Provider
```
app.provider('myProvider', function() {
        var name = "myProvider";
        return {
           //非$get返回内容，是唯一可以在config中访问的属性和方法
            setName: function(newName) {
                name = newName;
            },
            //$get返回内容，是唯一可以在控制器中访问的属性和方法
            $get: function() {
                function getName() {
                    return name;
                }
 
                return {
                    des: "This is provider service",
                    getName: getName
                };
            }
        };
    })
```

provider是唯一一种你可以传进 .config() 函数的service，factory和service都不可以。当你想要在 service 对象启用之前，先进行参数配置，那就应该用 provider。

注意：config是传入参数为provider的名称后面加provider。

```
.config(function(myProviderProvider) {
    myProviderProvider.setName('myProviderProvider');
});
```

参考资料：
http://www.tuicool.com/articles/rABfUvm
http://www.cnblogs.com/whitewolf/p/angular-services.html