---
layout: post
title:  "JavaScript如何判断变量的类型"
date:   2013-07-19 16:12:31
categories: JavaScript
---
当接收到一个变量需要判断其类型时，通常需要用typeof方法。

    typeof 1 // "number"  
    typeof true // "boolean"  
    typeof "str" // "string"  
    typeof [1, 2, 3] // "object"  
    typeof new Date() // "object"  
    typeof /\s/ // "object"  
    typeof new RegExp(/\s/) // "object"

可以看到当面对Array、RegExp等对象变量时，都会返回Object，此时可以用instanceof方法来判断对象的类型。

    [1, 2, 3] instanceof Array // true
    new Date() instanceof Date // true  
    /\s/ instanceof RegExp // true
    new RegExp(/\s/) instanceof RegExp // true

可是，这就需要逐个做一下判断，效率就有所降低了。此时可以利用Object原型链上的的toString方法，来将对象转化为"[object Xxx]"格式的字符串。

    Object.prototype.toString.call(true); // "[object Boolean]"  
    Object.prototype.toString.call(1); // "[object Number]"  
    Object.prototype.toString.call("str"); // "[object String]"  
    Object.prototype.toString.call([1, 2, 3]); // "[object Array]"  
    Object.prototype.toString.call(/\s/); // "[object RegExp]"  
    Object.prototype.toString.call(function(){}); // "[object Function]"

最后附上关于判断类型的整体代码。

    var type = function( obj ) {  
        var classArray = "Boolean Number String Function Array Date RegExp Object Error".split(" ")
            class2type = {};
        for (var i = classArray.length - 1; i >= 0; i--) {
            class2type[ "[object " + classArray[i] + "]" ] = classArray[i].toLowerCase();
        };
        if ( obj == null ) {
            return String( obj );
        }
        return typeof obj === "object" || typeof obj === "function" ?
            class2type[ Object.prototype.toString.call(obj) ] || "object" :
            typeof obj;
    };

    console.log(type(1)); // "number"  
    console.log(type(true)); // "boolean"  
    console.log(type("str")); // "string"  
    console.log(type([1, 2, 3])); // "array"  
    console.log(type(new Date())); // "date"  
    console.log(type(function(){})); // "function"  
    console.log(type(/\s/)); // "regexp"