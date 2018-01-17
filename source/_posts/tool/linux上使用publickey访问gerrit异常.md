---
title: linux上使用publickey访问gerrit异常
date: 2018-01-17 10:54:08
tags: [工具,gerrit]
---

### 现象
redhat上，先使用A账号，能正常执行相关操作。切换成B账号，上传public key后，执行异常：
```
Agent admitted failure to sign using the key.
Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
````
### 解决办法
执行完ssh-keygen再执行一下ssh-add
```
ssh-keygen -t rsa -C "mymail@zte.com.cn"
ssh-add ~/.ssh/id_rsa
```
