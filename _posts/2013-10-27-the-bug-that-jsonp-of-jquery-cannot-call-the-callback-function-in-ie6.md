---
layout: post
title:  "jQuery的JSONP在IE6下无法触发回调BUG"
date:   2013-10-27 02:15:15
categories: jQuery IE6 JavaScript
---
最近做一个PC的项目，需要支持到IE6，可是测试过程中发现在IE6下，jQuery的getJSON方法无法触发回调。这个真是让我百思不得其解啊，明明jQuery对浏览器兼容做的很好啊，为什么不能触发JSONP的回调呢？

在IE6下DEBUG是一件非常头疼的事情啊，经过各种尝试，发现原来在IE6下，浏览器的默认事件，比如：Submit、超链接等都会阻止回调函数的触发。

最后非常愉快的在回调函数中加上了一句话就全都解决了。

    e.preventDefault();

对IE6的兼容还真是一个永远都无法停下的话题。

UPDATE 2013-12-14:

在IE6下，即使在a标签链接中写上`javascript:void(0);`也一样会阻断掉包括JSONP以及AJAX请求。