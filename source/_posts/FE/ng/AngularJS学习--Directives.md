---
title: AngularJS学习--Directives
date: 2016-04-02 12:24:08
tags: [JS,AngularJS]
---
# Directives是什么？
AngularJS Directives是DOM元素(例如属性，元素名，注释或CSS类)上的标记，它告诉AngularJS的 html 编译器($compile) 把特定的操作连接到DOM元素或转化为DOM元素及其子元素，使元素拥有某些特定的行为。指令是所有AngularJS应用最重要的部分。尽管AngularJS已经提供了非常丰富的指令，但还是经常需要创建应用特定的指令。

## 什么时候需要用Directives？
不要滥用Directives，如下一些场合需要考虑使用：
1. 当某些元素或者元素组合以及其行为，需要在多个页面或者位置重复使用时；
2. 引用其他jQuery组件，并且在加载数据后需要进行初始化时；

##使用Directives的好处
1. 去除重复代码
2. 使得HTML更具可读性 例如：
```
//未使用指令时，无法知道该文本框的作用，需要查看JS代码。
<input type="text" />
//使用指令后，根据指令可得知是时间输入框。显然后者更清晰明了。
<input type="text" data-date-picker />
```
注意：如果不涉及DOM定义，也可以考虑使用serivces
# Directives有哪些形式？
AngularJS中有四种类型的自定义指令：
1. 元素指令  E 
```
<data-my-directive></ data-my-directive>
```
2. 属性指令   A 
```
<div data-my-attr></div>
```
3. CSS class 指令 C
```
<div class="my-class"></div>
```
4. 注释指令 M 
```
 <!– directive:my-comment -- >
```
# Directives的命名
如果要符合HTML5的规范，可以在元素前面添加 x- 或者 data-的前缀。在匹配指令的时候，Angular会在元素或者属性的名字中剔除 x- 或者 data- 前缀。 然后将 – 或者 : 连接的字符串转换成驼峰(camelCase)现形式，然后再与注册过的指令进行匹配。例如：
```
//指令定义
app.directive('helloWorld', function() {})

//指令应用
<div data-hello-world></div>
```
当然，如果不需要符合HTML5规范，不可以加x- 或者 data- 前缀，一样可以正常使用。

# Directives初体验
指令示例如下，
```
//指令定义
angular.module(‘app‘,[])
.directive('helloWorld', function() {
  return {
      restrict: 'AE',
      replace: 'true',
      template: '<h3>Hello World!!</h3>'
  };
});

//指令应用
<div hello-world></div>
```
## Directive的基本属性说明：
- restrict – 这个属性用来指定指令的类型，可用类型：A E C M。
- template – 这个属性规定了指令被Angular编译和链接后生成的HTML字符串。可以包含其他的指令，以及表达式({ { } })等。
- templateUrl—模板文件（template属性的HTML字符串保存成HTML文件）的URL地址，元素内容复杂的情况下推荐使用。
- replace – 这个属性指明生成的HTML内容是否会替换掉定义此指令的HTML元素。默认是false，templat内容直接插入到该元素里。
- transclude –说明指令是否复制原始标记中的内容。默认为false。注意：当前指令元素无子元素只有文本内容时，会自动添加span元素。
## Directive的属性——link：
如果我们要为DOM元素添加事件监听、监听模型属性变化、以及更新DOM等等行为时，就需要使用link属性。
```
link: function(scope, element , attrs)  {
        //dosomething
    }
```
参数说明：
- scope – 指令的scope。在我们的例子中，指令的scope就是父controller的scope。
- element – 指令的jQLite(jQuery的子集)包装DOM元素。如果你在引入AngularJS之前引入了jQuery，那么这个元素就是jQuery元素，而不是jQLite元素。由于这个元素已经被jQuery/jQLite包装了，所以我们就在进行DOM操作的时候就不需要再使用 $()来进行封装，可直接调用。 element .css('cursor', 'pointer');
- attrs  一个包含了指令所在元素的属性的标准化的参数对象。举个例子，你给一个HTML元素添加了一些属性，那么可以在 link 函数中通过 attrs.attributename 来使用它。

## Directive的属性——compile：
```
    compile: function(element, attrs) {
        //dosomething
    }
```
compile 函数在 link 函数被执行之前用来做一些DOM改造。它接收下面的参数：
- element – 指令所在的元素
- attrs– 元素上赋予的参数的标准化列表

要注意的是 compile 函数不能访问 scope，并且必须返回一个 link 函数。但是如果没有设置 compile 函数，你可以正常地配置 link 函数，（有了compile，就不能用link，link函数由compile返回）。

大多数的情况下，你只需要使用 link 函数。这是因为大部分的指令只需要考虑注册事件监听、监视模型、以及更新DOM等，这些都可以在 link 函数中完成。 但是对于像 ng-repeat 之类的指令，需要克隆和重复 DOM 元素多次，在 link 函数执行之前由 compile 函数来完成。这就带来了一个问题，为什么我们需要两个分开的函数来完成生成过程，为什么不能只使用一个？要回答好这个问题，我们需要理解指令在Angular中是如何被编译的。
### 指令是如何被编译的？
当应用引导启动的时候，Angular开始使用 $compile 服务遍历DOM元素。这个服务基于注册过的指令在标记文本中搜索指令。一旦所有的指令都被识别后，Angular执行他们的 compile 方法。如前面所讲的，compile 方法返回一个 link 函数，被添加到稍后执行的 link 函数列表中。这被称为编译阶段。如果一个指令需要被克隆很多次（比如 ng-repeat），compile函数只在编译阶段被执行一次，复制这些模板，但是link 函数会针对每个被复制的实例被执行。所以分开处理，让我们在性能上有一定的提高。这也说明了为什么在 compile 函数中不能访问到scope对象。 在编译阶段之后，就开始了链接（linking）阶段。在这个阶段，所有收集的 link 函数将被一一执行。指令创造出来的模板会在正确的scope下被解析和处理，然后返回具有事件响应的真实的DOM节点。

## Directive的属性——scope：
创建指令的作用范围，scope在指令中作为属性标签传递。scope 是创建可以复用指令的必要条件，每个指令（不论是处于嵌套指令的哪一级）都有其唯一的作用域，它不依赖于父scope。
scope与父scope的关系可以分为隔离和继承两种。
- 继承（默认就是继承）： scope: true,
- 完全隔离：scope :  {},
- 部分隔离： scope :  {name=‘@userName’}

非完全隔离场景下又存在如下3种继承关系：
```
scope: {
    name : '@',
    age : '=',
    doUpdate : '&'
}
```
- “@” 单向绑定，数值可以从父scope传递到子scope，反之则不行。
- “=” 双向绑定，数值可以在父scope，子scope之间互相传递
- “&“  引用，类似函数指针，使得子scope可以直接调用父scope函数，从而修改父scope的变量。
上述举例中，是属性和scope成员变量名称一致的情况。如果不一致，需要在符号后添加原始属性名称。例如：name:'@enName'

注意：指令应用时，继承的属性名称必须使用-连接单词，不能用驼峰命名。
```
<div my-directive do-update="doUpdate"><div>
```