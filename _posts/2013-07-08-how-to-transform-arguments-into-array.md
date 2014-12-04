---
layout: post
title:  "如何将类数组转化为数组"
date:   2013-07-08 12:58:15
categories: JavaScript
---
一些类数组，比如函数中的实际参数arguments对象，虽然与数组形似，但还不是一个真正的数组，有时候需要将其变成一个真正的数组。

现在可以用slice方法可以将arguments对象转化为一个真正的数组：

    var args = Array.prototype.slice.call(arguments, 0);

这也是jQuery中使用到的方法。

除此之外，也可以直接创建一个新数组来调用其slice方法实现：

    var args = [].slice.call(arguments, 0);

但这种方法经过测试效率并没有上面的方法高。

除了上面两种方法之外，也可以直接循环arguments对象，赋值到新的数组中：

    var args = [];  
    for (var i = 0, length = arguments.length; i < length; i++) {  
        args.push(arguments[i]);
    }

通常都是没有最优的方法，只有最合适的方法，大部分情况下，用第一种方法就可以实现对象到数组的转化，可是NodeList（比如：`document.getElementsByTagName("div")`）在IE中就不能使用第一种办法将其转化为数组，因为在JavaScript中，一个Dom NodeList并非一个JavaScript对象，在IE中再使用`var args = Array.prototype.slice.call(nodes, 0);`就会抛错，这时就需要用最原始的办法解决：

    function nodeListToArray(nodes) {  
        var arr;

        try {
            // works in every browser except IE
            arr = [].slice.call(nodes);
            return arr;
        } catch (err) {
            // slower, but works in IE
            arr = [];

            for (var i = 0, length = nodes.length; i < length; i++) {
                arr.push(nodes[i]);
            }
            return arr;
        }
    }