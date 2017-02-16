---
layout: post
title: 移动端解决方案
tags:
- H5
categories: 前端
description: 移动端字体单位font-size选择px还是rem
---

<!-- more -->
##移动端字体解决方案
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

##Touch事件的详细解析
当用户手指放在移动设备在屏幕上滑动会触发的touch事件
以下支持webkit
touchstart——当手指触碰屏幕时候发生。不管当前有多少只手指
touchmove——当手指在屏幕上滑动时连续触发。通常我们再滑屏页面，会调用event的preventDefault()可以阻止默认情况的发生：阻止页面滚动
touchend——当手指离开屏幕时触发
touchcancel——系统停止跟踪触摸时候会触发。例如在触摸过程中突然页面alert()一个提示框，此时会触发该事件，这个事件比较少用
TouchEvent
touches：屏幕上所有手指的信息
targetTouches：手指在目标区域的手指信息
changedTouches：最近一次触发该事件的手指信息
touchend时，touches与targetTouches信息会被删除，changedTouches保存的最后一次的信息，最好用于计算手指信息
参数信息(changedTouches[0])
clientX、clientY在显示区的坐标
target：当前元素
参考：https://developer.mozilla.org/en-US/docs/Web/API/TouchEvent


##Click事件的延时问题
移动设备上的web网页是有300ms延迟的，玩玩会造成按钮点击延迟甚至是点击失效。
原因就出在浏览器需要如何判断快速点击上，当用户在屏幕上单击某一个元素时候，例如跳转链接<a href="#"></a>，此处浏览器会先捕获该次单击，但浏览器不能决定用户是单纯要点击链接还是要双击该部分区域进行缩放操作，所以，捕获第一次单击后，浏览器会先Hold一段时间t，如果在t时间区间里用户未进行下一次点击，则浏览器会做单击跳转链接的处理，如果t时间里用户进行了第二次单击操作，则浏览器会禁止跳转，转而进行对该部分区域页面的缩放操作。那么这个时间区间t有多少呢？在IOS safari下，大概为300毫秒。这就是延迟的由来。造成的后果用户纯粹单击页面，页面需要过一段时间才响应，给用户慢体验感觉，对于web开发者来说是，页面js捕获click事件的回调函数处理，需要300ms后才生效，也就间接导致影响其他业务逻辑的处理。
解决方案：
fastclick可以解决在手机上点击事件的300ms延迟
zepto的touch模块，tap事件也是为了解决在click的延迟问题
触摸事件的响应顺序
1、ontouchstart 
2、ontouchmove 
3、ontouchend 
4、onclick
解决300ms延迟的问题，也可以通过绑定ontouchstart事件，加快对事件的响应。

##webkit表单输入框placeholder的颜色值能改变么
```
input::-webkit-input-placeholder{color:#AAAAAA;}
input:focus::-webkit-input-placeholder{color:#EEEEEE;}
```

其他：
禁止ios 长按时不触发系统的菜单，禁止ios&android长按时下载图片
```
.css{-webkit-touch-callout: none}
```

禁止ios和android用户选中文字
```
.css{-webkit-user-select:none}
```

##常用的移动端框架
zepto.js

语法与jquery几乎一样，会jquery基本会zepto~

最新版本已经更新到1.16

[官网](http://zeptojs.com/)

[中文(非官网)](http://www.css88.com/doc/zeptojs_api/)

常使用的扩展模块：

浏览器检测：https://github.com/madrobby/zepto/blob/master/src/detect.js

tap事件：https://github.com/madrobby/zepto/blob/master/src/touch.js

iscroll.js

解决页面不支持弹性滚动，不支持fixed引起的问题~

实现下拉刷新，滑屏，缩放等功能~

最新版本已经更新到5.0

[官网](http://cubiq.org/iscroll-5)

underscore.js

笔者没用过，不过听说好用，推荐给大家~

该库提供了一整套函数式编程的实用功能，但是没有扩展任何JavaScript内置对象。

最新版本已经更新到1.8.2

[官网](http://underscorejs.org/)

滑屏框架

适合上下滑屏、左右滑屏等滑屏切换页面的效果

slip.js

iSlider.js

fullpage.js

swiper.js
