---
layout: post
title: 移动端解决方案
tags:
- H5
categories: 前端
description: 移动端字体单位font-size选择px还是rem
---

<!-- more -->
## 媒体查询
body{font-family:Helvetica;}
移动端字体单位font-size选择px还是rem
对于只需要适配少部分手机设备，且分辨率对页面影响不大的，使用px即可
对于需要适配各种移动设备，使用rem，例如只需要适配iPhone和iPad等分辨率差别比较挺大的设备
rem配置参考，适合视觉稿宽度为750px的：
可以借鉴手淘的[解决方案](http://lib.csdn.net/article/html5/42085)媒体查询参考
```
html{font-size:10px}
@media screen and (min-width:321px) and (max-width:375px){html{font-size:11px}}
@media screen and (min-width:376px) and (max-width:414px){html{font-size:12px}}
@media screen and (min-width:415px) and (max-width:639px){html{font-size:15px}}
@media screen and (min-width:640px) and (max-width:719px){html{font-size:20px}}
@media screen and (min-width:720px) and (max-width:749px){html{font-size:22.5px}}
@media screen and (min-width:750px) and (max-width:799px){html{font-size:23.5px}}
@media screen and (min-width:800px){html{font-size:25px}}
```
## js控制
```
var oHtml = document.getElementsByTagName("html")[0];
var scWid = document.documentElement.offsetWidth||document.body.offsetWidth;
var initWid = 750;
if(scWid>750){
	scWid=750;
}
oHtml.style.fontSize = 100*scWid/initWid + "px";
```
这段js将px转换成rem，如果ui设计稿是按iphone设计的，initWid就等于750.