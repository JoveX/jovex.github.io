---
layout: post
title:  "Firefox诡异现象：margin被诡异添加"
date:   2013-10-27 04:52:04
categories: Firefox Html/Css 浏览器
---
最近写页面的时候用firefox测试过程中发现firefox的一个怪异现象，不知道是否算是firefox的一个bug。

先打开你的火狐看一个示例。（[示例1](http://jser.io/demos/firefox-margintop-one.html)）

​可以看到当前外层元素的margin和padding已经设置为0了。

![Firefox诡异现象：margin被诡异添加](/userfiles/images/firefox-margintop-one-1.png)

里层元素只设置了一个宽和高将父元素顶开，并且让其浮动且清除浮动。

![Firefox诡异现象：margin被诡异添加](/userfiles/images/firefox-margintop-one-2.png)

此时奇迹出现了，竟然外层元素设置的margin-bottom，同时也加到了其margin-top上，可是明明外层元素没有设置margin-top啊，真是百思不得其解啊。

再看第二个示例：（[示例2](http://jser.io/demos/firefox-margintop-two.html)）

![Firefox诡异现象：margin被诡异添加](/userfiles/images/firefox-margintop-two-1.png)

![Firefox诡异现象：margin被诡异添加](/userfiles/images/firefox-margintop-two-2.png)

![Firefox诡异现象：margin被诡异添加](/userfiles/images/firefox-margintop-two-3.png)

其实示例2与示例1大体一样，只是将外层元素的margin-bottom去掉，并在外层元素下多加了一个带有margin-top的元素，此时奇迹出现了，竟然新加元素的margin-top也叠加到了外层元素的上面。

PS：这些不是外边距叠加问题，可以同时在包括IE在内的浏览器里面查看效果。真是百思不得其解啊...