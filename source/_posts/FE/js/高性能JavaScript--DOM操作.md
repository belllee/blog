---
title: 高性能JavaScript--DOM操作
date: 2017-12-12 12:24:08
tags: [JS,性能]
---

用JS进行DOM操作的代价是昂贵的，它是富web应用中最常见的性能瓶颈。
### DOM
文档对象模型（DOM）是一个独立于语言的，用于操作XML和HTML文档的程序接口（API）。通常在浏览器中DOM和JS都是独立的，因为彼此独立，所以JS操作DOM，性能开销就很大。
### 提升性能最佳实践
* 减少操作
典型场景，循环操作DOM，改成循环拼接，最后操作DOM。
```
//循环操作DOM15000次
function innerHTMLLoop(){
	for(var count = 0; count < 15000; count++){
		document.getElementById('here').innerHTML += 'a';
	}
}
//只操作1次DOM，在IE8中性能提升273倍
function innerHTMLLoop(){
	var content = '';
	for(var count = 0; count < 15000; count++){
		content += 'a';
	}
	document.getElementById('here').innerHTML = content;
}
```
* 增加新元素时innerHTML性能高于DOM方法
```
var newDiv = "<div></div>";
document.getElementById('here').innerHTML = newDiv;
//
var newElement = document.createElement('div');
document.getElementById('here').appendChild(newElement);
```
* 使用节点clone
```
var newElement1 = document.createElement('div');
var newElementN = newElement1.cloneNode(true);
```

* HTML集合使用array代替
DOM查询方法（getElementByName，getElementByClassName，getElementByTagName），以及部分属性（images,links,forms,elements）返回值是HTML集合（类似数组，有length和下标访问，无push，slice）。这些集合要避免重复访问，因为每次访问都会重新执行查询。
```
//由于每次添加div后，长度增加，此循环是死循环
var divList= document.getElementByName('div')；
for(var count = 0; count < divList.length; count++){
	document.body.appendChild(document.createElement('div'));
}
//集合保存到变量，避免重复查询
for(var count = 0; count < document.getElementByName('div').length; count++){
	//do something
}
var divList= document.getElementByName('div')；
var len = divList.length
var arr = toArray(divList); //toArray是自定义的函数，将集合转成array
for(var count = 0; count < len; count++){
	//do something
}
```
注意：此方法会额外增加一次遍历操作，长度小的集合可能不会提升反而下降

* 局部变量替代
```
for(var i = 0; i < document.getElementsByTagName("a").length; i++){
		document.getElementsByTagName("a")[i].class = 'active'
}
//改进后
var list = document.getElementsByTagName("a");
var len = list.length;
for(var i = 0; i < len; i++){
		list[i].class = 'active'
}
```
* IE6、7中遍历DOM，nextSibling性能高于childNode
* 遍历元素节点优选元素节点属性
childNodes,firstChild和nextSibling这些属性并不区分元素节点和其他类型节点（比如注释和文本节点）。如果只需要查询元素节点，优先使用如下方法代替。（注意：IE6-8只支持children属性）

|元素节点属性名|被替代的属性名|
| - | :-: | -: | 
|children|childNodes|
|childElementCount|childNodes.length|
|firstElementChild|firstChild|
|lastElementChild|lastChild|
|nextElementSibling|nextSibling|
|previousElementSibling|previousSibling|
*  利用CSS选择器提高查找效率
querySelectorAll()方法使用CSS选择器作为参数，并且返回匹配节点的类数组对象。不会返回HTML集合，不会对应实时的文档结构，避免了之前讨论的HTML集合引起的性能和逻辑问题。代码示例如下：
```
//改进前
var els = document.getElementsById("menu").getElementsByTagName("a");
//改进后
var els = document.querySelectorAll("#menu a");
```
如果是组合查询，querySelectorAll()方法更具优势。对比一下：
```
//改进前
var els = [];
var divs = document.getElementsByTagName("div");
var className = "";
for(var i = 0,len = divs.length; i<len;i++){
	className = divs[i].className;
	if(className === 'warning' || className === 'notice'){
		els.push(divs[i]);
	}
}
//改进后
var els = document.querySelectorAll("div.warning, div.notice");
```
推荐使用querySelector()方法，查询第一个匹配的节点。

## 重绘与重排
浏览器下载完所有的组件文件（html、js、css、图片）之后，会解析这些组件，并生成两个数据结构：
1. DOM树：表示页面结构
2. 渲染树：表示DOM节点如何显示
DOM树中的每一个需要显示的节点在渲染树中至少存在一个对应的节点（隐藏的DOM元素在渲染树中没有对应的节点）。渲染树中的节点被称为“帧”或者“盒”，具有内边距padding，外边距margin，边框border和位置position（IE盒模型的高度和宽度包括边框和内边距，W3C只是内容部分。W3C盒模型可使用box-sizing:border-box改成IE盒模型）。一旦DOM树和渲染树构建完成，浏览器就开始显示（绘制）页面元素。
当DOM的变化影响了元素的几何属性（宽和高），浏览器需要重新计算元素的集合属性，同事其他元素的集合属性和位置也会受到影响。浏览器会使渲染树中受到影响的部分失效，并重新构造渲染树。这个过程被称为“重排reflow”。完成重排后，浏览器会重新绘制受影响的部分到平路中，该过程被称为“重绘repaint”。
并不是所有的DOM变化都会影响几何属性，比如改变背景色，此时只会执行重绘而不会触发重排。重绘和重排都是代价昂贵的操作，需要尽量避免。  

### 触发重排的操作
1. 添加和删除可见的DOM元素
2. 元素位置改变
3. 元素尺寸变化
4. 内容改变
5. 页面渲染器初始化
6. 浏览器窗口尺寸改变
7. 滚动条的出现和消失会触发整个页面的重排

### 渲染树变化的排队与刷新
由于重排消耗大，大多数浏览器都会通过队列化修改并批量执行来优化重排过程。获取布局的如下操作会导致队列刷新：
*  offsetTop，offsetLeft，offsetWidth，offsetHeight
* scrollTop，scrollLeft，scrollWidth，scrollHeight
* clientTop，clientLeft，clientWidth，clientHeight
* getCumputedStyle()(currentStyle in IE)
执行这些属性和方法需要返回最新的布局信息，因此浏览器会执行渲染队列中的操作，已获得最新的布局信息。因此不需要避免频繁执行这些属性和方法。
### 最小化重排和重绘
为减少重排或者重绘，应该合并多次对DOM和样式的修改，然后一次性处理。
#### 修改样式
```
//优化前,执行了三次重排。大部分现代浏览器进行了优化，可能只执行一次
var el = document.getElementById('mydiv');
el.style.borderLeft='1px';
el.style.borderRight='2px';
el.style.padding='5px';
//优化后，只执行一次
var el = document.getElementById('mydiv');
el.style.ccsText='border-left:1px;border-right:2px;padding:5px;';
//第二种优化方法
var el = document.getElementById('mydiv');
el.className='active';
```
#### 批量修改DOM
可以通过如下步骤减少重绘和重排次数：
1. 使元素脱离文档流
2. 对其应用多重改变
3. 把元素待会文档中
这样操作后只会在1和3补执行两次重排，忽略了步骤2中可能的N次重排。
使元素脱离文档流的方法有如下三种：
1. 隐藏元素，应用修改，重新显示
2. 使用文档片段在当前DOM之外构建一个子树，执行完修改后再把它拷贝回文档
3. 讲原始元素拷贝到一个脱离文档的节点中，修改这个副本，完成后再替换原始元素。
```
function appendDataToElement(appendToElement, data) {
	var a, li;
	for (var i = 0, max = data.length; i < max; i++) {
		a = document.createElement('a');
		a.href = data[i].url;
		a.appendChild(document.createTextNode(data[i].name));
		li = document.createElement('li');
		li.appendChild(a);
		appendToElement.appendChild(li);
	}
};
//优化前,循环内N次重排
var ul = document.getElementById('mylist');
appendDataToElement(ul, data);
//第一种，异常和显示
var ul = document.getElementById('mylist');
ul.style.display = 'none';
appendDataToElement(ul, data);
ul.style.display = 'block';
//第二种，文档片段
var fragment = document.createDocumentFragment();
appendDataToElement(fragment, data);
document.getElementById('mylist').appendChild(fragment);
//第三种，元素替换
var old = document.getElementById('mylist');
var clone = old.cloneNode(true);
appendDataToElement(clone, data);
old.parentNode.replaceChild(clone, old);
```
推荐第二种方案，其产生的DOM遍历和重排次数最少。
### 缓存布局信息
当查询布局信息（例如offsets,scroll,client等），浏览器为返回最新值，会刷新队列并应用所有变更。所哟尽量减少布局信息的获取次数，获取后赋值给局部变量，然后再操作局部变量。
例如移动元素的例子，timeout循环部分
```
//改进前
myElement.style.left = 1 + myElement.offsetLeft + 'px';
myElement.style.top = 1 + myElement.offsetTop + 'px';
if (myElement.offsetLeft >= 500) {
	stopAnimation();
}
//改进后，先一次性获取初始位置
var current = myElement.offsetLeft;
//然后循环执行操作
current++
myElement.style.left = current + 'px';
myElement.style.top = current + 'px';
if (current >= 500) {
	stopAnimation();
}
```
### 让元素脱离动画流
采用绝对位置定位，可以减少元素尺寸变化时，对其他元素造成的重排影响。
例如折叠/展开这种交互方式，每次变化都会导致下方所有元素的移动。如果把这部分元素使用绝对位置定位，覆盖其他部分。这样就能避免下方元素的重排和重绘，减少开销。
### IE和:hover
从IE7开始，IE允许任何元素上使用:hover这个CSS伪选择器。如果大量使用:hover，响应速度下降明显。特别是IE8。
### 事件委托
如果进行大量的DOM元素事件绑定，会引入性能问题。一个简单的解决方案是事件委托。只需给最外层的元素绑定事件，利用事件逐层冒泡并能被父级元素捕获，就可以处理所有子元素上触发的事件。
