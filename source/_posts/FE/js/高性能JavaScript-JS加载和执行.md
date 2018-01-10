---
title: 高性能JavaScript--JS加载和执行
date: 2018-01-10 21:28:07
tags: [JS,性能]
---
随着前端技术的不断发展，JS在web应用中的作用越来越来大，同时也意味着JS越来越成为前端的瓶颈。写出高性能的JS代码，也成为了前端开发者必备的技能。这里分享一下《高性能JavaScript》的读书心得，以便大家更好的理解JS，写出更高效的JS代码。  
## JS加载和执行  
### JS的加载  
1. 地球人都知道，为了不阻塞页面的加载和渲染，JS文件一般放在html的底部加载。
2. 减少加载的JS脚本数量，一般采用合并文件的方式
3. 减小JS文件大小，压缩JS文件，开启GZIP压缩  

### 无阻塞的脚本  
避免一次加载一个很大的JS文件，避免阻塞浏览器。主要采用延迟加载技术，具体实现方法包括：
1. 利用script标签的延迟加载属性defer(需注意兼容性，可以在[](http://caniuse.com/)上查看)
```
<script type="text/javascript" src="file1.js" defer></script>
```
带有defer属性的script标签，可以放在文档的任何位置。对应的JS文件将在script标签解析时下载，但代码不会被执行，直到DOM加载完成（onload事件触发之前）才会执行。JS文件下载时不会阻塞页面的其他处理过程。
2. 动态创建script元素,通过src加载脚本 
```
var script= document.createElement ("script");
script.type= "text/javascript";
script.src= "file1.js";
document.getElementsByTagName("head")[0].appendChild(script)
```
特点：无论在何处启动下载，文件的下载和运行都不会阻塞页面的其他处理过程。如需控制狂态和加载顺序，需要自己添加监听事件处理.
```
//Internet Explorer
script.onreadystatechange =function(){...}
//Firefox,Opera, Chrome,Safari3+
script.onload = function(){...}
```  

3. XMLHttpRequest脚本注入，通过XHR获取JS字符串，填入script元素。
```
var xhr= newXMLHttpRequest();
xhr.open("get","file1.js",true);
xhr.onreadystatechange= function(){
    if (xhr.readyState==4){
        if (xhr.status>=200&&xhr.status<300|| xhr.status== 304){
            varscript= document.createElement("script");
            script.type= "text/javascript";
            script.text= xhr.responseText;
            document.body.appendChild(script);
        }
    }
};
xhr.send(null)
```
特点：下载后可以不用立即添加script元素，从而控制执行时间。
### 推荐的无阻塞模式
先加载必要的JS文件，然后再执行load函数，再加载其他功能所需的脚本。
```
<script type="text/javascript" src="loader.js"></script>
<script type="text/javascript">
    loadScript("the-rest.js",function(){
        Application.init();
    });
</script>
```
目前各主流的延迟加载类库，也是采用这些技术。比如，图片懒加载，动态给img元素的src赋值。requireJS是使用动态创建script元素实现的。  
