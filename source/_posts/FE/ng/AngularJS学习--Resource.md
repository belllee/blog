---
title: AngularJS学习--Resource
date: 2016-05-09 14:24:08
tags: [JS,AngularJS]
---
AngularJS提供了$resourceService来更方便地与RESTful服务进行交互，可以方便地定义一个REST资源，而不必手动声明CRUD方法。

# 如何使用Resource？
具体步骤如下：
1. 引入angular-resource.min.js文件 
2. 在模块中依赖ngResourece,
3. 在服务中注入$resource

示例代码如下：
```
//定义
var Topic = angular.module('Topic',['ngResource']);
Topic.factory('topicResource',function($resource){
  return $resource('/topic/:id',
    {
      id:'@id'
    },
    {
      close:{
        method:'POST',
        params:{close:true},
        isArray:false
      }
    }
  );
});

//调用,对id为123的topic执行close操作
topicResource.close({id:123}, successFn, errorFn)
```
# $resource的参数说明
$resource(url,{url参数},{自定义方法})
1. url: 必填, 资源的基础url，url中带有':'项的是根据第二个参数来进行配置的。
2. url参数: 选填，配置url中的带有':' 项的参数。例如: ('/card/user/:userID/:id', {userID:123,id:'@id'}), 那么userID会被配置为123.。另外,在调用$resource()的方法的时候(比如get,query...),可以传入参数覆盖这里对url参数的配置。
3. 自定义方法: 选填，使用$resource获取到的资源,或者通过$resource实例化的资源,资源本身会具有一些方法,比如$save,第三个参数用于给资源添加自定义的方法。

# $resource的方法
$resource()默认有如下5个方法:
1. get: {method:'GET'}  一般用于获取某个资源.
2. query: {method:'GET',isArray:true},一般用于获取一整套的资源,isArray参数，用以设置返回数据格式，是否是数组。默认是true。
3. save: {method:'POST'},一般用于保存某个资源,有可能是新建的资源,也有可能是更新现有的资源
4. remove: {method:'DELETE'},一般用于删除某个资源
5. delete:  {method:'DELETE'} , 一般用于删除某个资源

# $resource的响应方法
 Resource.action([parameters], [success], [error])
1. [parameters]: 可选. 一个json对象,用于配置url里的参数,比如这里写了{id:cardID},那么提交的请求url就是 '/card/user/123/cardID'. 可以不填,不填就直接按照$resource()里的url来提交,注意,不填的话,不需要给个空,可以直接写success回调,angular能够判断出它没有填第一个参数,而不是死板的按照顺序来解读参数.
2. [success]:可选. 请求成功后的回调函数.回调接受2个参数(注意这里和$http有所不同)function(data, responseHeaders)
3. [error]:可选. 请求失败后的回调.回调接受1个参数 function(httpResponse){   }
注意：凡是通过$resource返回的对象,一定是json格式的,如果后台返回的数据不是json,$resource也会按照自己的方式处理成json格式.不能返回null,因为$resource必须返回json格式,所以要返回{}
#响应转换
有时基于既定的后台设计，无法提供完全RESTful的API，比如返回的是一个分页器对象，而非数组。此时，我们仍然可以使用$resource，但需要设置响应转换回调。例如：
```
/*
API返回的是一个分页对象，需要读取其中的数组数据。
{
  currentPage : 1,
  totalPage : 20,
  pageSize : 2,
  content : [
    {
      id : 1,
      body : 'hello'
    },
    {
      id : 2,
      body : 'world'
    }
  ]
}
*/

//定义
var Topics = $resouce('/topic/list/page/:pageid', null, { 
  page: {
    method : 'GET',
    transformResponse: function(data, headers){
      var pager = JSON.parse(data);
      return pager.content;
    }    
  }
});

//调用
Topics.page(function(data){
  //此时data已经是转换过的数组了
});
```
当然上述转换，也可以在响应函数中进行相关的数据格式处理。

类似响应重写，你还可以设置请求转换transformRequest。