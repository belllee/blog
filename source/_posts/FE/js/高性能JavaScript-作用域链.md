---
title: 高性能JavaScript--作用域链
date: 2018-01-10 21:28:35
tags: [JS,性能]
---
## 数据存取
JS中有如下四种基本数据的存取：
- 字面量：字符串、数字、布尔值、对象、数组、函数、正则表达式、null和undefined。
- 本地变量：var/let 定义的数据存储单元。
- 数组元素
- 对象成员
通常情况下，访问速度排序：字面量 > 本地变量 > 数组元素 > 对象成员。个别浏览器的版本，可能有细微差别。
### 作用域
执行环境/运行期上下文(execution context): 是指当前变量或函数有权访问的其它数据。每个执行环境都有一个与之关联的变量对象(variable object, VO)，VO是不能直接访问的，执行环境中定义的所有变量和函数都会保存在这个对象中，解析器在处理数据的时候就会访问这个内部对象。
全局执行环境是最外层的一个执行环境，在web浏览器中全局执行环境是window对象，因此所有全局变量和函数都是作为window对象的属性和成员函数创建的。每个函数都有自己的执行环境，当执行流进入一个函数的时候，函数的环境会被推入一个函数栈中，而在函数执行完毕后执行环境出栈并被销毁，保存在其中的所有变量和函数定义随之销毁，控制权返回到之前的执行环境中，全局的执行环境在应用程序退出（浏览器关闭）才会被销毁。
每一个JS函数可以看做是Function对象的一个实例，并且含有一个内部属性[[Scopes]],[[Scopes]]包含了一个函数被创建的作用域中对象的集合。这个集合被称为作用域链，它决定哪些数据能被函数访问。函数作用域中的每个对象被称为一个可变对象，每个可变对象都是以“key-value”形式存在。
典型的作用域链：
1. 函数创建时
此时函数的作用域链会压入第一个作用域对象，即创建此函数的作用域中可访问的数据对象填充。如下图所示：

![scope1.png](http://upload-images.jianshu.io/upload_images/4944427-48ee5069cc4bba9e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注意：这个作用域对象是可变的，可以理解为这个对象是引用的。
具体对象信息可以在chrome dev tool中查看，如下图：

![scope1-tool.png](http://upload-images.jianshu.io/upload_images/4944427-2f9ebf872328209d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 函数执行时
每次执行函数时都会创建一个执行环境，每个执行环境都是独一无二的，多次调用函数就会导致创建多个执行环境。此时会将会将一个被称为“活动对象”(activation object,AO)的新对象作为第二个作用域对象压入作用域链。如下图所示:

![scope2.png](http://upload-images.jianshu.io/upload_images/4944427-9709f477eae42387.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在函数的执行过程中，每遇到一个变量或者函数，都会在作用域链中按照顺序进行查找，直到遍历所有的作用域，此过程会影响运行性能。
3. 闭包时的作用域
如下一段代码中，包含了闭包
```
functionassignEvents(){
    var id= "xdi9592";
    document.getElementById("save-btn").onclick =function(event){
        saveDocument(id);
    };
}   
```

![scope3.png](http://upload-images.jianshu.io/upload_images/4944427-98c7ce04672a331c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![scope3-tool.png](http://upload-images.jianshu.io/upload_images/4944427-b5a1093ace42448b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


4. 其他改变作用域的情况
一般作用域链的顺序是按照调用的顺序排列的，但是特殊情况下会改变。
- with
执行with语句时，会将with带入的对象压入作用域链，导致调用深度发生变化。
```
functioninitUI(){
    with(document){
        var bd= body,
        links=getElementsByTagName_r("a"),
        i=0,
        len= links.length;
        while(i<len){
        update(links[i++]);
        }
        getElementById("go-btn").onclick=function(){
        start();
        };
        bd.className ="active";
    }
}
```
![with.png](http://upload-images.jianshu.io/upload_images/4944427-71b9617f98d995b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- try-catch
类似with，当try块发生异常时，程序跳转到catch子句，并且把异常对象压入作用域首位。catch子句执行完作用域链恢复之前的状态。由于加深了调用深度，如果在catch子句执行操作会造成性能问题。可以采用错误处理函数的方式，改变作用域链的状态，从而减少调用深度。
```
try{
    methodThatMightCauseAnError();
}catch (ex){
    handleError(ex);//delegate tohandlermethod
}
```
### 标识符解析的性能
在执行环境的作用域链中，一个标识符的位置越深，他的读写速度就越慢，因此函数中读写局部变量是最快的，读写全局变量通常是最慢的。
改进办法，通过赋值给局部变量，改变标识符的深度，从而提高读写速度。
```
for(var i = 0; i < document.getElementsByTagName("a").length; i++){
    document.getElementsByTagName("a")[i].class = 'active'
}
//改进后
var list = document.getElementsByTagName("a");
for(var i = 0; i < list.length; i++){
    list[i].class = 'active'
}
```