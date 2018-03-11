---
title: WEB前端介绍
date: 2016-10-16 19:39:06
tags: [前端]
---
## **1、WEB前端是神马**

Web前端开发是从网页制作演变而来的，名称上有很明显的时代特征。在互联网的演化进程中，网页制作是Web1.0时代的产物，那时网站的主要内容都是静态的，用户使用网站的行为也以浏览为主。2005年以后，互联网进入Web2.0时代，各种类似桌面软件的Web应用大量涌现，网站的前端由此发生了翻天覆地的变化。网页不再只是承载单一的文字和图片，各种富媒体让网页的内容更加生动，网页上软件化的交互形式为用户提供了更好的使用体验，这些都是基于前端技术实现的。 以前会Photoshop和Dreamweaver就可以制作网页，现在只掌握这些已经远远不够了。无论是开发难度上，还是开发方式上，现在的网页制作都更接近传统的网站后台开发，所以现在不再叫网页制作，而是叫Web前端开发。Web前端开发在产品开发环节中的作用变得越来越重要，而且需要专业的前端工程师才能做好。Web前端开发是一项很特殊的工作，涵盖的知识面非常广，既有具体的技术，又有抽象的理念。简单地说，它的主要职能就是把网站（Web App网页应用和Hybrid App混合应用）的界面更好地呈现给用户。

## **2、前端技术**

### **W3C标准**

W3C标准不是某一个标准，而是一系列标准的集合。网页主要由三部分组成：结构（Structure）、表现（Presentation）和行为（Behavior）。对应的标准也分三方面：结构化标准语言主要包括HTML，表现标准语言主要包括CSS，行为标准主要包括对象模型（如W3C DOM）、ECMAScript等。

简单来说就是： html（结构） + css（表现） + js（行为）

学习资源：[http://www.w3school.com.cn/](http://www.w3school.com.cn/) 
名词解释：W3C——万维网联盟（World Wide Web Consortium）

### **HTML——WEB应用的基石**

HyperText Markup Language 超文本标记语言。超文本是用超链接的方法，将各种不同空间的信息组织在一起的网状文本。信息可以包含图片、链接，甚至音乐、程序等非文字元素。

### **html文件示例如下：**
![](https://upload-images.jianshu.io/upload_images/4944427-b0e4fe78a10f1245.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### **标签的基本格式如下：**
![](https://upload-images.jianshu.io/upload_images/4944427-200c7bc7b8a12f73.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

示例中的标签是个闭合标签，有部分标签不是闭合标签。例如：<input type="text" name="test" value="test"/>

一个标准的html文件主要包括如下常用的标签：

- !DOCTYPE:文档类型声明，它的目的是要告诉浏览器，它应该使用什么样的文档类型定义来解析文档。示例中所示的文档类型是html5。文档类型一般是向下兼容的，html5基本兼容html4.01.
- html：文档的根节点。<html> 与 </html> 标签限定了文档的开始点和结束点，在它们之间是文档的头部和主体。
- head：<head> 标签用于定义文档的头部，它是所有头部元素的容器。<head> 中的元素可以引用脚本、指示浏览器在哪里找到样式表、提供元信息等等。
- meta：<meta> 元素可提供有关页面的元信息（meta-information），比如针对搜索引擎的关键词，文件编码类型，浏览器版本等。
- title：文档标题，浏览器的标签页显示的内容。
- link：<link>标签定义文档与外部资源的关系，最常见的用途是链接样式表（css文件），ICON。
- script：<script>标签用于定义客户端脚本，比如 JavaScript。既可以包含脚本语句，也可以通过 src 属性指向外部脚本文件。
- style：<style> 标签用于为 HTML 文档定义样式信息。
- body:body 元素定义文档的主体。
- h1~h6：<h1> 定义最大的标题。<h6> 定义最小的标题。
- div：<div> 可定义文档中的分区或节，可以把文档分割为独立的、不同的部分
- p：<p> 标签定义段落。
- span：<span> 标签被用来组合文档中的行内元素。
- a:<a> 标签定义超链接，用于从一张页面链接到另一张页面，最重要的属性是 href 属性，它指示链接的目标。
- button：<button> 标签定义一个按钮。
- img：img 元素向网页中嵌入一幅图像。
- input：<input> 标签用于搜集用户信息。根据不同的 type 属性值，输入字段拥有很多种形式。输入字段可以是文本字段、复选框、单选按钮、按钮等等。

其他更多标签，请见：[http://www.w3school.com.cn/tags/tag_doctype.asp](#/_blank)

### **文档流**
![](https://upload-images.jianshu.io/upload_images/4944427-cc38e0d5778d1508.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

冷知识：W3C规范中没有document flow这个概念，只有normal-flow, 文档流的叫法主要还是多数中文译者的翻译方式问题。据说来源于报纸的排版。

什么是文档流？简单说就是元素按照其在HTML中决定标签位置顺序排布的过程。并且这种过程遵循标准的描述。html的文档流规则是将窗体自上而下分成一行行, 并在每行中按从左至右的顺序排放元素。

### **块级标签和内联标签**

HTML中的标签主要是块级标签或内联标签。块级标签一般是其他标签的容器标签，能容纳其他块标签或内联标签。简单来说，块标签就好比一个四方块，可以放其他的四方块，并可以呈现在页面上任何地方。默认情况下块标签，是独占一行的。常见的块标签：div、h1-h6标题、form（只能用来容纳其他块标签）、hr、p、table、ul、ol等。内联标签也叫内嵌标签或行内标签，一般都是基于语义级(semantic)的基本标签。内联标签只能容纳文本或者其他内联标签，常见内联标签有a和span。 

块标签与内联标签的区别？ 
1. 块级标签，总是在新行上开始；内联标签，和其他标签都在一行上。
2. 块级标签，能容纳其他块标签或内联标签；内联标签，只能容纳文本或者其他内联标签。 
3. 块级标签中高度，行高以及顶和底边距都可控制；内联标签中高，行高及顶和底边距不可改变。

在实际应用中，可以通过CSS修改标签的display属性转变标签的性质，进行相互转换。建议按照原始性质使用，尽量不进行转换。

### **如何设计html？**

如下图，通常先设计好结构图，然后根据结构图再进行标签的书写。
![](https://upload-images.jianshu.io/upload_images/4944427-b5704c57de05413d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## **HTML5**

HTML4.01版本诞生于 1999 年。自从那以后，Web 世界已经经历了巨变。多媒体的大量应用，各类app不断涌现。不同于HTML4.01完全由W3C制定。HTML5 是 W3C 与 WHATWG 合作的结果。WHATWG 致力于 web 表单和应用程序，而 W3C 专注于 XHTML 2.0。HTML5草案的前身名为 Web Applications 1.0，于2004年被WHATWG提出，于2007年被W3C接纳，并成立了新的 HTML 工作团队。HTML5 的第一份正式草案已于2008年1月22日公布。最终在2014年定稿。

注：W3C 指 World Wide Web Consortium，万维网联盟。
注：WHATWG 指 Web Hypertext Application Technology Working Group，Web超文本应用技术工作组。

### **HTML5建立的一些规则：**
1. 新特性应该基于 HTML、CSS、DOM 以及 JavaScript。
2. 减少对外部插件的需求（比如 Flash）
3. 更优秀的错误处理
4. 更多取代脚本的标记
5. HTML5应该独立于设备

### **HTML5中的新特性**
1. 用于绘画的 canvas 元素
2. 用于多媒体的 video 和 audio 元素
3. 对本地离线存储的更好的支持
4. 新的特殊内容元素，比如 article、footer、header、nav、section
5. 新的表单控件，比如 calendar、date、time、email、url、search

### **浏览器支持**
最新版本的 Chrome、Firefox、Safari 以及 Opera 基本上完全支持 HTML5 特性。IE9开始支持某些 HTML5 特性。详细对比：
[http://caniuse.com/#comparison](#/_blank)  
[http://html5test.com/compare/browser/firefoxmobile-35/chrome-39/ie-8.html](#/_blank)

### **HTML4.01和HTML5结构区别**
 ![](https://upload-images.jianshu.io/upload_images/4944427-86ac4ec9e255f77a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### **HTML5新特性简单示例**
### **canvas**
canvas标签使用 JavaScript 在网页上绘制图像。画布是一个矩形区域，您可以控制其每一像素。canvas 拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法。
代码示例：
```
<!DOCTYPE html>
 <html>
     <body>
         <canvas id="myCanvas" width="200" height="100" style="border:1px solid #c3c3c3;">
             Your browser does not support the canvas element.
         </canvas>
         <script type="text/javascript">
             var c=document.getElementById("myCanvas");
             var cxt=c.getContext("2d");//指定二维绘图，目前只支持2D绘图，后续可能会支持3D
             cxt.moveTo(10,10);//绘制起点。（0，0）对应左上角，向右和向下递增
             cxt.lineTo(150,50);//画一条直线（10,10）到（150,50）
             cxt.lineTo(10,50);//画一条直线（150,50）到（10,50）
             cxt.strokeStyle="green"; //指定线条颜色
             cxt.stroke();//显示绘图结果
         </script>
     </body>
 </html>
```
### **vedio/audio**
在HTML5以前，如果您想在网页中嵌入在线观看的视频，一般都需要使用Flash视频流，通过使用 <object> 和 <embed> 标签，就可以通过浏览器播放swf、flv等格式视频文件，但是前提是浏览器必须安装第三方插件：Adobe Flash Player。而现代智能手机和iPad等一般都不支持Flash，所以无法浏览网页上的视频。而 HTML 5 改变了这一事实，Web开发者只需要使用 <video> 标签就可以轻松加载视频文件，而不需要任何第三方插件。

vedio支持的视频格式有三种：
1. Ogg  带有 Theora 视频编码和 Vorbis 音频编码的 Ogg 文件
2. MPEG4（MP4）带有 H.264 视频编码和 AAC 音频编码的 MPEG 4 文件
3. WebM  带有 VP8 视频编码和 Vorbis 音频编码的 WebM 文件

代码示例：
```
 <video id="coures-video" class="vjs-tech" preload="none" 
     poster="http://demo.com/upload/course/20160104/1451886552971.jpg"
     controls="controls" loop="loop" width="640px" height="480px;"> 
      <source src="http://demo.com/upload/course/20160104/1451886551972.mp4" type="video/mp4">
 </video>
```
audio与vedio基本相同,不再赘述。

### **Drag 和 Drop**
拖放是一种常见的特性，即抓取对象以后拖到另一个位置。在 HTML5 中，拖放是标准的一部分，任何元素都能够拖放。只需要简单的几个属性和响应函数即可实现拖放。

代码示例：
```
<div id="div1" ondrop="drop(event)" ondragover="allowDrop(event)">
     <img src="http://demo.com/images/11.jpg" draggable="true"     ondragstart="drag(event)" id="drag1" />
 </div>
 <div id="div2" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
```  
```
 //ondragover 事件规定在何处放置被拖动的数据。
 function allowDrop(ev){
     ev.preventDefault();
 }
 //移动响应事件，获取正在移动的标签ID
 function drag(ev){
     ev.dataTransfer.setData("Text",ev.target.id);
 }
 //进行放置 响应事件
 function drop(ev) {
     ev.preventDefault();
     var data=ev.dataTransfer.getData("Text");
     ev.target.appendChild(document.getElementById(data));
 }
```
### **INPUT新类型**
HTML5 拥有多个新的表单输入类型，这些新特性提供了更好的输入控制和验证。email，url，number，range，Date pickers (date, month, week, time, datetime, datetime-local)，search，color 。
- email 类型用于应该包含 e-mail 地址的输入域，在提交表单时，会自动验证 email 域的值：
    <input type="email" name="user_email" />
- number 类型用于应该包含数值的输入域，还能够设定对所接受的数字的限定：
   <input type="number" name="points" min="1" max="10" />
- Date Pickers（日期选择器）拥有多个可供选取日期和时间的新输入类型：
date - 选取日、月、年
month - 选取月、年
week - 选取周和年
time - 选取时间（小时和分钟）
datetime - 选取时间、日、月、年（UTC 时间）
datetime-local - 选取时间、日、月、年（本地时间）
 <input type="date" name="user_date" />

### **本地存储**
说到本地存储，真是历尽千辛万苦才走到HTML5这一步，之前的历史大概如下图所示：
![](https://upload-images.jianshu.io/upload_images/4944427-e930aa1ef64979c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最早的Cookies大小约4KB，而且IE6只支持每个域名20个cookies。优势就是大家都支持，而且支持得还比较好。后来IE又推出了userData，现在用的比较多的是Flash，空间是Cookie的25倍，基本够用。再之后Google推出了Gears，虽然没有限制，但是要装额外的插件。到了HTML5把这些都统一了，官方建议是每个网站5MB。所有支持的浏览器目前都采用的5MB，尽管有一些浏览器可以让用户设置，但对于网页制作者来说，目前的形势就5MB来考虑是比较适合的。

对于重复的数据，HTML5可以直接从本地存储中读取数据，不必每次都向服务器发起请求从服务器获取。从而大幅提高网站性能和支持离线运行。对于不同的网站，数据存储于不同的区域，并且一个网站只能访问其自身的数据。

HTML5 提供了两种在客户端存储数据的方法：
1. localStorage - 没有时间限制的数据存储
2. sessionStorage - 针对一个 session 的数据存储

HTML5 使用 JavaScript 来存储和访问数据。使用方法如下：
```
if (localStorage.pagecount){ 
    localStorage.pagecount=Number(localStorage.pagecount) +1;
 } else {
     localStorage.pagecount=1;
 }
 document.write("Visits: " + localStorage.pagecount + " time(s).");
```
## **CSS**
层叠样式表 (Cascading Style Sheets)是一种用来表现HTML或XML等文件样式的计算机语言。CSS目前最新版本为CSS3，是能够真正做到网页表现与内容分离的一种样式设计语言。相对于传统HTML的表现而言，CSS能够对网页中的对象的位置排版进行像素级的精确控制，支持几乎所有的字体字号样式，拥有对网页对象和模型样式编辑的能力，并能够进行初步交互设计，有较强的易读性，被称为程序员的画笔。
### **如何使用样式表？**

有三种方法可以在网页上使用样式表：

1.  外联式Linking（也叫外部样式）
当样式需要被应用到很多页面的时候，外部样式表将是理想的选择。使用外部样式表，你就可以通过更改一个文件来改变整个站点的外观。
```
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```
2. 嵌入式Embedding（也叫内页样式）
当单个文件需要特别样式时，就可以使用内部样式表。你可以在 head 部分通过 <style> 标签定义内部样式表。
```
<head>
  <style type="text/css"> 
    body {background-color: red} 
    p {margin-left: 20px}
  </style>
</head>
```
3. 内联式Inline（也叫行内样式）
当特殊的样式需要应用到个别元素时，就可以使用内联样式。 使用内联样式的方法是在相关的标签中使用样式属性。样式属性可以包含任何 CSS 属性。以下实例显示出如何改变段落的颜色和左外边距。
```
<p style="color: red; margin-left: 20px"> 
This is a paragraph 
</p>
```
### **如何定义样式表**

CSS 规则由两个主要的部分构成：选择器，以及一条或多条声明。如下图所示

![](https://upload-images.jianshu.io/upload_images/4944427-df91f0bb24f4066a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### **选择器类型：**
1. 标签选择器 p  <p></p>
2. 类选择器 .my-class  <p class="my-class"></p>
3. ID选择器 #myDiv  <div id="myDiv"></div>
4. 属性选择器 [type="radio"] <input type="radio" />
5. 伪类/伪元素选择器 a:hover
6. 以及上述各种选择器的组合。

更多介绍，详见：[http://www.w3school.com.cn/cssref/css_selectors.asp](#/_blank)

### **盒模型**
网页设计中常听的属性名：内容(content)、填充(padding)、边框(border)、边界(margin)。这些属性我们可以把它转移到我们日常生活中的盒子（箱子）上来理解，日常生活中所见的盒子也就是能装东西的一种箱子，也具有这些属性，所以叫它盒子模式。CSS盒子模型就是在网页设计中经常用到的CSS技术所使用的一种思维模型。盒模型的模型如下图
![](https://upload-images.jianshu.io/upload_images/4944427-57b0c88cb9d4c137.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/4944427-1ba2ba1a7fb9259f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### **display属性**
display 属性规定了元素应该生成的框的类型。在上面html章节中我们提到了块级标签和内联标签，这只是默认属性，通过CSS中的display进行修改。还可以通过display控制标签是否显示。
display常见的属性值如下：
| 值 | 描述|
|--|--|
|none|此元素不会被显示。 |
|block|此元素将显示为块级元素，此元素前后会带有换行符。|
|inline|默认。此元素会被显示为内联元素，元素前后没有换行符。 |
|inline-block|行内块元素。（CSS2.1 新增的值）|
|table|此元素会作为块级表格来显示（类似 <table>），表格前后带有换行符。|

### **position属性**

position 属性规定元素的定位类型。这个属性定义建立元素布局所用的定位机制。任何元素都可以定位，不过绝对或固定元素会生成一个块级框，而不论该元素本身是什么类型。相对定位元素会相对于它在文档流中的默认位置偏移。
| 值 | 描述 |
|--|--|
| absolute | 生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。|
| fixed | 生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
|relative | 生成相对定位的元素，相对于其正常位置进行定位。因此，"left:20" 会向元素的 LEFT 位置添加 20 像素。 |
| static | 默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）。 |
| inherit | 规定应该从父元素继承 position 属性的值。 |

示例：
![](https://upload-images.jianshu.io/upload_images/4944427-41ae02b45dade8ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/4944427-40d3d157ccdd74d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
参考资料：[http://blog.jobbole.com/49320/](#/_blank)

### **float属性**
float 属性定义元素在哪个方向浮动。以往这个属性总应用于图像，使文本围绕在图像周围，不过在 CSS 中，任何元素都可以浮动。浮动元素会生成一个块级框，而不论它本身是何种元素。如果浮动非替换元素，则要指定一个明确的宽度；否则，它们会尽可能地窄。假如在一行之上只有极少的空间可供浮动元素，那么这个元素会跳至下一行，这个过程会持续到某一行拥有足够的空间为止。
|值|描述| 
|--|--| 
|left|元素向左浮动。|
|right|元素向右浮动。 |
| none| 默认值。元素不浮动，并会显示在其在文本中出现的位置。 |
| inherit | 规定应该从父元素继承 float 属性的值。|

示例：

- 正常文档流
![](https://upload-images.jianshu.io/upload_images/4944427-aa0d8c1aa75d3d99.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 设置float，浏览器的宽度“不够长”时 ![图片18.png](https://upload-images.jianshu.io/upload_images/4944427-423465a304018193.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 设置float，浏览器的宽度“足够长”时
![图片19.png](https://upload-images.jianshu.io/upload_images/4944427-7a9ebd1861bad959.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


参考资料：[http://www.cnblogs.com/polk6/archive/2013/07/25/3142187.html](#/_blank)

### **浮动子标签撑开父标签**

子标签如果都设置为float，会导致父标签没有高度值。导致结构错乱。

如下两种方法可以解决：

1. 添加一个空的div，设置clear属性为both
![](https://upload-images.jianshu.io/upload_images/4944427-bc7177b5998354f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2. 父标签增加伪类，设置clear属性为both
![图片21.png](https://upload-images.jianshu.io/upload_images/4944427-c2171dea0bb5eeec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### **其他常用属性**
```
 background-color: gray;  /*背景颜色*/
border: 2px solid #e4e4e4; /*边框尺寸、类型、颜色*/
 font-family: 宋体;  /*字体*/
color: #234567; /*文字颜色*/
font-size: 13px;  /*文字大小*/
width: 70px;  /*宽度*/
height: 70px;  /*高度*/
```
### **CSS优先级**
由于CSS选择器可以有多种形式，可以继承，可以重复定义。所以具体哪个样式生效需要计算优先级。注意：优先级相同时，晚定义的样式有效。
通常情况下，给每个选择器设置一个数值，数值越大，优先级越高。实际没有这个数值，只是一种近似。
html标签 （1）
class  （10）
id  （100）
计算示例：
div p     1+1 = 2
span.test 1+10 = 11
#id.test     100 + 10 = 110
不同引入方式引入的CSS，优先级也不同。排序规则如下：!important > inline >id > class > tag

### **CSS3**
CSS3是CSS技术的升级版本，CSS3语言开发是朝着模块化发展的。以前的规范作为一个模块实在是太庞大而且比较复杂，所以，把它分解为一些小的模块，更多新的模块也被加入进来。这些模块包括： 盒子模型、列表模块、超链接方式 、语言模块 、背景和边框 、文字特效 、多栏布局等。

参考：[http://www.w3school.com.cn/css3/](#/_blank)

## **javascript**

**java ≠ javascript  就像 马 ≠ 海马**

JavaScript是一种属于网络的脚本语言,已经被广泛用于Web应用开发,常用来为网页添加各式各样的动态功能,为用户提供更流畅美观的浏览效果。JavaScript是甲骨文公司的注册商标，最初由Netscape的Brendan Eich设计。Netscape在最初将其脚本语言命名为LiveScript，后来Netscape在与Sun合作之后将其改名为JavaScript。JavaScript最初受Java启发而开始设计的，目的之一就是“看上去像Java”，因此语法上有类似之处，一些名称和命名规范也借自Java。但JavaScript的主要设计原则源自Self和Scheme。JavaScript与Java名称上的近似，是当时Netscape为了营销考虑与Sun微系统达成协议的结果。

JavaScript脚本语言具有以下特点:
1. 脚本语言。JavaScript是一种解释型的脚本语言,C、C++等语言先编译后执行,而JavaScript是在程序的运行过程中逐行进行解释。
2. 基于对象。JavaScript是一种基于对象的脚本语言,它不仅可以创建对象,也能使用现有的对象。
3. 简单。JavaScript语言中采用的是弱类型的变量类型,对使用的数据类型未做出严格的要求,是基于Java基本语句和控制的脚本语言,其设计简单紧凑。
4. 动态性。JavaScript是一种采用事件驱动的脚本语言,它不需要经过Web服务器就可以对用户的输入做出响应。在访问一个网页时,鼠标在网页中进行鼠标点击或上下移、窗口移动等操作JavaScript都可直接对这些事件给出相应的响应。
5. 跨平台性。JavaScript脚本语言不依赖于操作系统,仅需要浏览器的支持。因此一个JavaScript脚本在编写后可以带到任意机器上使用，部署成本小。

不同于服务器端脚本语言，例如PHP与ASP，JavaScript主要被作为客户端脚本语言在用户的浏览器上运行，不需要服务器的支持。所以在早期程序员比较青睐于JavaScript以减少对服务器的负担，而与此同时也带来另一个问题：安全性。

而随着服务器的强壮，虽然程序员更喜欢运行于服务端的脚本以保证安全，但JavaScript仍然以其跨平台、容易上手等优势大行其道。同时，有些特殊功能（如AJAX）必须依赖Javascript在客户端进行支持。随着引擎如V8和框架如Node.js的发展，及其事件驱动及异步IO等特性，JavaScript逐渐被用来编写服务器端程序。

JavaScript被Netscape公司提交给ECMA(European Computer Manufacturers Association)制定为标准，称之为ECMAScript，标准编号ECMA-262。目前最新版为2015年发布的ECMAScript 6。

下面对javascript的常用操作进行举例：

### **写入 HTML 输出**
```
document.write("<h1>This is a heading</h1>");
document.write("<p>This is a paragraph</p>");
```
### **对事件作出反应**
```
<button type="button" onclick="alert('Welcome!')">点击这里</button>
```
alert() 函数在 JavaScript 中并不常用，但它对于代码测试非常方便。
onclick 鼠标点击事件。

### **改变 HTML 内容**
```
x=document.getElementById("demo")  //查找元素
x.innerHTML="Hello JavaScript";    //改变内容
var img=document.getElementById("myImg")  //查找元素
img.src="/i/eg_bulbon.gif";
```
您会经常看到 document.getElementByID("some id")。这个方法是 HTML DOM 中定义的。DOM（文档对象模型）是用以访问 HTML 元素的正式 W3C 标准。

### **改变 HTML 样式**
```
x=document.getElementById("demo")  //找到元素
x.style.color="#ff0000";           //改变样式
```
### **验证输入**
```
if isNaN(x) {alert("Not Numeric")};
```
详细的语言特性，参考：[http://www.w3school.com.cn/js/index.asp](#/_blank)

### **jQuery**

JQuery一个优秀的轻量级的Javascript库。它兼容CSS3，还兼容各种浏览器（IE 6.0+, FF 1.5+, Safari 2.0+, Opera 9.0+），jQuery2.0及后续版本不再支持IE6/7/8浏览器。jQuery使用户能更方便地处理HTML、events、实现动画效果，并且方便地为网站提供AJAX交互。jQuery还有一个比较大的优势是，它的文档说明很全，而且各种应用也说得很详细，同时还有许多成熟的插件可供选择。目前，绝大多数网站都在使用jQuery。

示例：
```
$("#h01").attr("style","color:red").html("Hello jQuery")
```
详细的语言特性，参考：[http://www.w3school.com.cn/jquery/](#/_blank)

## **浏览器兼容**

浏览器兼容性问题又被称为网页兼容性或网站兼容性问题，指网页在各种浏览器上的显示效果可能不一致而产生浏览器和网页间的兼容问题。在网站的设计和制作中，做好浏览器兼容，才能够让网站在不同的浏览器下都正常显示。因为不同浏览器使用内核及所支持的HTML，JS，CSS标准不同；以及用户客户端的环境不同（如分辨率不同）造成的显示效果不能达到理想效果。最常见的问题就是网页元素位置混乱，错位，动态效果失效等等。

举例：
1. IE的盒模型与W3C标准不符。
2. IE8不支持html5的vedio，只能使用flash播放视频。
3. IE8不支持图片圆角
4. DOM 个别操作不一致
```
var obj = document.getElementById('objId');
obj.setAttribute('onclick','funcitonname();'); //FIREFOX支持，IE不支持 
obj.onclick=function(){fucntionname();}; //都支持
```
## **前端调试工具**

推荐使用Chrome devtools。

## **关于UX和前端**

在前端开发过程中一般会引入比较成熟的前端框架，例如：Bootstarp（[http://v3.bootcss.com/getting-started/](#/_blank)）。所以在UI设计时需要进行沟通，一旦选择了框架，尽量能和框架风格保持一致。当然也可以完全重新定义风格和自己开发。

同样在图标使用上，一般也会选择成熟的图标库，例如：Font Awesome，Glyphicons。（[http://www.bootcss.com/p/font-awesome/](#/_blank)）

上面提到的兼容性设计问题。如果要支持IE8这类古董浏览器。再设计时很多新特性都无法支持，如果一定要支持，成本会很大。
