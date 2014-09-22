---
layout: post
title:  "监听输入框的即时变化oninput & onpropertychange"
date:   2013-03-26 12:41:00
categories: HTML5 oninput onpropertychange Javascript
---
之前在做一个监听文本框的字符个数，对输入字数做限制的功能。

开始想单纯的监听用户的keydown/keypress/keyup等一系列键盘的动作，可是用户的输入多种多样，比如粘贴/剪切等，输入法也是无法完美监听到的。

后来想到用change,可是change事件必须要失去焦点才能触发。

查询了一些资料后发现，input和propertychange几乎可以满足所有的需求。

input是作为HTML5的标准事件出现的，除IE6/7/8外，包括IE9的其他所有标准浏览器全部都已经实现了其方法。

那么对于IE6/7/8就可以写成如下形式：

  
    dom.attachEvent('onpropertychange', function() {  
        // do something
    });

可是，如果用dom.setAttribute('data-value', 'value')这样的操作，也会触发propertychange事件，那么就需要对event.propertyName做一下判断。

  
    dom.attachEvent('onpropertychange', function() {  
        if (window.event.propertyName == "value") {
            // do something
        }
    });

在包括IE9的所有标准浏览器下，直接监听input就可以，如下：

  
    dom.addEventListener('input', function() {  
        // do something
    }, false);

因为propertychange在IE9下已经取消了，所以对dom的对象搜索一下是否有改方法或属性就可以了的。

下面是完整的监听方法：
  
    function (dom, callback) {  
        if ("onpropertychange" in dom) { //IE6/7/8
            dom.attachEvent('onpropertychange', function() {
                if (window.event.propertyName == "value") {
                    callback.call(this, window.event);
                }
            }
        } else {
            dom.addEventListener("input", callback, false);
        }
    }