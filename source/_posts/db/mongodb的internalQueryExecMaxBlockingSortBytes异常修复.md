---
title: mongodb的internalQueryExecMaxBlockingSortBytes异常修复
date: 2018-01-10 21:57:40
tags: [db,mongodb]
---
### 现象
node执行的服务出现异常，查看日志发现如下错误。
```
MongoError: QueryFailure flag set on getmore command
    at Object.toError (e:\code\api\mobile\nodejs\node_modules\mongodb\lib\mongodb\utils.js:114:11)
    at e:\code\api\mobile\nodejs\node_modules\mongodb\lib\mongodb\cursor.js:854:31
    at e:\code\api\mobile\nodejs\node_modules\mongodb\lib\mongodb\db.js:1905:9
    at Server.Base._callHandler (e:\code\api\mobile\nodejs\node_modules\mongodb\lib\mongodb\connection\base.js:453:41)
    at e:\code\api\mobile\nodejs\node_modules\mongodb\lib\mongodb\connection\server.js:488:18
    at MongoReply.parseBody (e:\code\api\mobile\nodejs\node_modules\mongodb\lib\mongodb\responses\mongo_reply.js:68:5)
    at .<anonymous> (e:\code\api\mobile\nodejs\node_modules\mongodb\lib\mongodb\connection\server.js:446:20)
    at emitOne (events.js:96:13)
    at emit (events.js:188:7)
```
查询该错误，未找到具体原因。然后在mongodb\cursor.js 854行，增加打印result。得到具体错误信息如下：
Overflow sort stage buffered data usage exceeds in internal limit
mongo执行sort语句时，内存最大32M，如果数据量大，超过这个限制就出抛出异常。

### 解决办法
1、给sort语句中的字段建立索引。
  比如： sort({ endDate: -1, createTime: -1})
  建立索引如下：db.activity.createIndex({ endDate: -1, createTime: -1})  。其中acitivity是集合名

2、增加内存限制
需要在admin数据库下role为root的账户下设置,例如设置成100M
```
use admin
db.auth("adminuser","passwd")
db.adminCommand({setParameter: 1, internalQueryExecMaxBlockingSortBytes: 104857600})
```
综合查询性能和服务器资源占用，推荐使用建立索引的方式。

### 其他需要注意的地方
除了sort, aggregate也存在内存限制，这是需要使用allowDiskUse参数，允许使用硬盘缓存中间数据。具体设置如下
```
db.activity.aggregate(
		[{ $unwind: '$applyment' },
		{ $match: { 'applyment.items.value': req.query.uid }},
		{ $project : {id:1,title: 1, 'applyment.approve': 1,'applyment.createTime': 1, endDate: 1}},
		{ $sort:{ 'applyment.createTime': -1 }},
		{ $skip:(result.pageNumber - 1) * result.pageSize},
		{ $limit:result.pageSize}],
		{ allowDiskUse: true}）
```
### 关于设置索引
1、mongoDB 3.0开始ensureIndex被废弃，今后都仅仅是db.collection.createIndex的一个别名。
2、子对象的属性设置索引db.activity.createIndex({ "applyment.createTime": -1}) 
3、数组内置顶位置设置索引db.activity.createIndex({ "applyment.items.0.value": 1}) 
