---
layout: post
title:  "用CSS让背景图片拉伸铺满"
date:   2013-06-03 08:22:36
categories: Html/Css 背景铺满
---
最近需要让背景图片在div中拉伸铺满，上网查了几篇文章，方法做一下记录。

在高级浏览器用到了css3的background-size属性和在IE下的滤镜filter。

因为在IE下滤镜filter中的AlphaImageLoader会将图片解析成背景图片，所以需要在IE下将背景图片设置为none。

    .bg-cover {
        background: url('bg.jpg') no-repeat;
        background-size: 100% 100%;
        background-image: none\9;
        filter: progid:DXImageTransform.Microsoft.AlphaImageLoader(src='bg.jpg', sizingMethod='scale');
    }