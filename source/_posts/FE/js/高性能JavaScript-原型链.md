---
title: 高性能JavaScript--原型链
date: 2018-01-10 21:29:12
tags: [JS,性能]
---
### 对象成员
前面已经介绍过，访问对象成员的速度比字面量或变量要慢，某些浏览器比数组元素还要慢。这里所说的对象成员包括属性和方法。大部分的JS代码都是以面向对象风格编写的，这就导致了非常频繁的访问对象成员操作？
#### 原型
JS的对象是基于原型的。原型是其他对象的基础，它定义并实现了一个新创建的对象所必须包含的成员列表。不同于其他编程语言的类，原型为所有对象实例所共享，因此这些实例也共享原型对象的成员。并且每个对象实例上的原型修改后，会影响原型的定义。
在Firefox，Safari，Chrome和IE11+浏览器里，对象实例可以通过__proto__读取原型对象。一旦创建一个内置对象（例如Object或Array）的实例，他们就会自动拥有一个Object对象作为原型。
对象可以有两种成员类型：实例成员（或own成员）和原型成员。实例成员直接存在于对象实例中，原型成员则从对象的原型继承而来。实例代码如下：
```
function Book(title, publisher){
    this.title = title
    this.publisher = publisher
}
var book1 = new Book("High Performance JavaScript","Yahoo! Press");
console.log(book1.title);//实例成员
console.log(book1.toString());//原型成员
```
如何判断对象的相关成员是否存在？是实例成员还是原型成员成员？方法如下：
```
console.log( book1.hasOwnProperty('title')) //true
console.log( book1.hasOwnProperty('toString')) //false
console.log('title' in book1) //true
console.log('toString' in book1) //true
```
#### 原型链
一个对象实例的原型对象，如果不是Object对象，那么原型对象就还有其自己的原型对象，直到原型对象是Object对象才结束。这种原型对象的嵌套就是原型链。
Object实例的原型就是Object对象，而其他对象生成实例时，instance.__proto__.__proto__才是Object对象。
示例代码：
```
function Book(title, publisher){
    this.title = title
    this.publisher = publisher
}
Book.prototype.price = 1.2

var book1 = new Book("High Performance JavaScript","Yahoo! Press");
```
实例book1的原型(__proto__)是Book.prototype,而Book.prototype的原型是Object。
对象Book拥有__proto__和prototype两个属性，
注意prototype的使用，只有在实例中才能直接读取prototype定义的属性。
```
function seven() {
    this.a = 7;
}
seven.prototype.a = -1;
console.log('seven.a', seven.a); //undefine
console.log('new seven().a', new seven().a); //7
```

#### 缓存对象成员值
访问对象成员时，对象在原型链的位置越深，读取的速度也就越慢。只有在必要时才使用对象成员，特别是没有必要反复多次读取同一对象成员。最佳做法是将属性值保存在局部变量中，使用局部变量代替属性以避免多次查找带来的性能开销。
实例代码如下：
```
function toggle(element){
    if(YAHOO.util.Dom.hasClass(element,'selected')){
        YAHOO.util.Dom.removeClass(element,'selected');
        return false;
    } else {
        YAHOO.util.Dom.addClass(element,'selected');
        return true;
    }
}

//改进后
function toggle(element){
    var dom = YAHOO.util.Dom;
    var hasClass = dom.hasClass(element,'selected');
    if(hasClass){
        dom.removeClass(element,'selected');
    } else {
        dom.addClass(element,'selected');
    }
    return !hasClass;
}
```
注意：
这种优化技术，并不推荐用于对象的成员方法。因为许多对象方法使用this来判断执行环境，把一个对象方法保存在局部变量会导致this的改变，从而导致异常。示例如下：
```
function Book(title, publisher){
    this.title = title
    this.publisher = publisher
    this.getTitle = function(){
        return this.title
    }
}

var book1 = new Book("High Performance JavaScript","Yahoo! Press")
console.log(book1.getTitle())
var mytitle = book1.getTitle
console.log(mytitle()) //throw exception

```