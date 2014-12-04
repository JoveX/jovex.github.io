---
layout: post
title:  "JavaScript中两个叹号的作用"
date:   2013-07-07 01:52:50
categories: JavaScript
---
在一些JavaScript代码中，经常能看到两个单号(!!)这样的写法，比如如下jQuery源码中grep方法：

    grep: function(elems, callback, inv) {  
        var ret = [],
            retVal;
        inv = !! inv;

        // Go through the array, only saving the items
        // that pass the validator function
        for (var i = 0, length = elems.length; i < length; i++) {
            retVal = !! callback(elems[i], i);
            if (inv !== retVal) {
                ret.push(elems[i]);
            }
        }

        return ret;
    }

那么两个叹号的作用是什么呢？我将上面的的代码改写成这样：

    inv = ! ( ! inv ) ;

如果inv是undefinded，那么! inv就会变成true，再加一个叹号，就会变成false。

所以两个叹号的作用就是明确inv只能在ture或false中取值，即将inv转化为布尔类型。

因为在JavaScript中，||或&&操作符并不是简单的返回true或false，所以将参与运算的值转化为布尔值，也为以后判断提供便利。