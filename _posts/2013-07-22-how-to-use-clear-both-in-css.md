---
layout: post
title:  "Css清除浮动"
date:   2013-07-22 13:41:18
categories: Html/Css
---
网页布局过程中，用到最多的就是浮动了，所以清除浮动就是一个需要考虑的问题，同时在IE6下的各类Bug也相当的让人头疼啊。

最初其实是在浮动元素的父元素后加一个没有任何意义的空标签，让其清除浮动。

    <div class="some-float-elem"></div>
    <div class="clearfix"></div>
    <style type="text/css">
    .clearfix {
        clear: both;
    }
    </style>

如果这样写，会在页面中增加很多无意义的空标签，这对前端工程师来说简直就是灾难，而且还会在IE6在造成“躲猫猫”的bug。

之后学到，应该像如下的写法，并将此样式加到所有浮动元素的父标签上就OK，在此mark一下：

    /*清除浮动*/
    .clearfix:after{
        content:"\0020";
        height:0;
        display:block;
        clear:both;
        visibility:hidden;
    }
    .clearfix {
        *zoom:1;
    }

可以看到在高级浏览器中，其实就是将该父元素后添加一个点(.)，然后将其隐藏，并清除浮动。这样就可以不用再添加那么多没有任何意义的空标签了的，HTML代码也变得整洁很多，同时也避免了IE6下的Bug。而在低版本浏览器中，仅仅是将zoom设置为1就可以了。