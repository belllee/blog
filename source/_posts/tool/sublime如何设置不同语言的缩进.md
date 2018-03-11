---
title: sublime如何设置不同语言的缩进
date: 2015-12-09 19:39:06
tags: [工具]
---
首先用sublime打开一个文件，比如要设置java语言的缩进，就打开一个java文件。

菜单栏: Preferences -> Settings - More -> Syntax Specific - User，打开属性编辑页面，录入如下内容。保存即可
```
{
    'tab_size': 2,
    'translate_tabs_to_spaces': true 
}
```

注意：tab_size表示tab键转成几个空格，请根据自己团队的变成规范来设置。

该设置文件会保存在如下路径\Sublime Text 2.0.2\Data\Packages\User。文件名为语言名+“.sublime-settings”后缀。比如java语言的配置文件就是Java.sublime-settings。