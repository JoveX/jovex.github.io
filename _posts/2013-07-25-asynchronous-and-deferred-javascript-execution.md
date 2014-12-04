---
layout: post
title:  "JavaScript中Asynchronous和Deferred的执行"
date:   2013-07-25 15:06:01
categories: Html/Css HTML5 JavaScript
---
在HTML的script标签中，浏览器允许自己定义JavaScript在什么时候开始执行。

下图是从[http://peter.sh/experiments/asynchronous-and-deferred-javascript-execution-explained/](http://peter.sh/experiments/asynchronous-and-deferred-javascript-execution-explained/)拿来的，非常直观的解释了defer和async两个属性执行的情况。

![JavaScript中Asynchronous和Deferred的执行](/userfiles/images/execution2.jpg)

#####正常执行&lt;script&gt;

就是默认的script行为，在执行js代码的过程中浏览器会停止对html的渲染，这对于较慢的服务器以及繁重的js脚本，浏览器的渲染就会被推迟，浏览器就会显示出一个空白的页面。

#####延迟执行&lt;script defer&gt;

即推迟js在html都加载完毕之后再执行，只是目前为止有些浏览器还不支持。

#####异步执行&lt;script async&gt;

只要不考虑加载顺序的话可以考虑这个方法，并且在js加载完毕之后会立即执行。