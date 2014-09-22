---
layout: post
title:  "CSS实现背景半透明"
date:   2013-04-13 05:11:00
categories: Html/Css 半透明
---
半透明一直以来都是前端工程师的困扰，不但要针对IE做兼容，还有各种特殊情况。

CSS的半透明，可能大家都会想到利用如下的写法：

    opacity: 0.5; /* FF */  
    filter:alpha(opacity=50); /*IE*/

可是如果这么写的话，虽然可以实现背景的半透明，但其子元素也都变成了半透明的，这个不是我们想要的。

有些时候就会对半透明单独做一个层，可是就会令代码比较混乱，不好整理。

这时，CSS3的rgba+IE的filter可以完美解决这个问题，并且都不会被继承。

    .alpha{
        background:rgba(0, 0, 0, 0.5);
        filter:progid:DXImageTransform.Microsoft.Gradient(GradientType=0, StartColorStr='#7f000000', EndColorStr='#7f000000');
    }

可是这么写的话，发现IE9透明度叠加了，这是因为IE9两个属性都支持造成的。利用IE9单独的hack，再将其中一个属性变为不透明即可。

    .alpha {
        filter:progid:DXImageTransform.Microsoft.gradient(enabled='true',startColorstr='#7F000000', endColorstr='#7F000000');
    }
    :root .alpha {
        filter:none;
        background-color:rgba(0,0,0,0.5);
    }

目前为止就全部搞定，背景透明，而且子元素不透明了。

最后附上一个计算16进制的公式，以便对filter计算不同透明度的16进制值。

    Math.floor(0.5 * 255).toString(16);