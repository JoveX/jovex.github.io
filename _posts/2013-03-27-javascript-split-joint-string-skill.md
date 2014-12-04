---
layout: post
title:  "JavaScript字符串拼接技巧"
date:   2013-03-27 04:38:00
categories: JavaScript
---
前端工程师常常会将很长一段的html填充相应的数据后插入到页面上。

比如有一个自定义的提示框，需要填充进相应的数据显示在用户界面：
  
    <div class='alertPanel'>
        <span>提示信息</span>
        <input type='button' class='okButton' />
    </div>

那么就需要前端工程师将很长的一段html拼接成字符串后放到页面上。

传统的方法就是将全部的代码写成一行，可是这样代码就已经超出一屏了，可读性非常的不好，后来就想到用加号（+）换行来拼接，如下：
  
    var str = "<div class='alertPanel'>"  
         + "<span>" + data.message + "</span>"
         + "<input type='button' class='okButton' />"
     + "</div>";

可是这个方法很容易出错，之后想到用数组的join方法来拼接字符串，把每一行作为数组的一项放到数组中，然后利用join方法将数组变成字符串。

  
    var str = ["<div class='alertPanel'>",  
        "<span>" + data.message + "</span>",
        "<input type='button' class='okButton' />",
    "</div>"].join("");

这样出错的概率能降低一些，同时可读性也能继续保持。