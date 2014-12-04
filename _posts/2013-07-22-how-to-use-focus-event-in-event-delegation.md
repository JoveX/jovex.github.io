---
layout: post
title:  "事件代理中如何使用focus等事件"
date:   2013-07-22 10:11:43
categories: JavaScript
---
通常在需要大量的绑定事件的时候，需要用到事件代理。事件代理即将事件绑定到所有需要绑定事件的父节点，比如document，这样当目标节点发生任何相关操作（比如点击），该事件就会冒泡到父级节点，就会触发绑定在document上的事件，此时用target属性来判断事件促发的目标节点来执行相应的函数操作。可是诸如load、change、submit、focus、blur等事件，并不支持事件冒泡，所以并无法使用事件代理。

那么换个思路，使用事件捕获，让其在事件捕获过程中触发。

    dom.addEventListener('focus', focusHandler, true);

那么在IE下应该如何解决该问题呢？因为低版本IE并不支持事件的捕获。那么此时就可以使用onfocusin和onfocusout事件来代替，其与focus类似，不过支持事件的冒泡。现在jQuery中，绑定事件过程中，也会自动将focus转为focusin事件。

    dom.onfocusin = focusHandler;  
    dom.onfocusout = blurHandler;

最后可以将两个方法放在一起，做一个hover的方法：

    if (document.addEventListener) {  
        dom.addEventListener('focus', focusHandler, true);
        dom.addEventListener('blur', blurHandler, true);
    } else {
        dom.onfocusin = focusHandler;
        dom.onfocusout = blurHandler;
    }