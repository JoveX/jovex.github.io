---
layout: post
title:  "JavaScript实现div拖放功能"
date:   2013-03-27 17:22:00
categories: 拖放 Javascript
---
拖放已经成为当今非常流行的界面模式。越来越多的web应用都做成桌面的形式，那么少不了窗口拖放这一功能。

拖放的概念非常的简单，在一个目标上按住鼠标，在另一个指定的地方松开鼠标，将目标放在这里。

其实拖放的思想非常简单，就是将一个元素设为绝对定位，然后根据鼠标的位置改变其偏移量，达到拖放的效果。

拖放的整个过程可以分成三个部分：按下鼠标（mousedown）、鼠标移动（mousemove）、松开鼠标（mouseup）。

移动的过程，就是监听鼠标的mousemove事件，那当鼠标移动时，让目标元素的偏移量与鼠标的位置一样，代码如下：

  
    EventUtil.addEventHandler(document, "mousemove", function(event) {  
        var _event = event || window.event;
        var J_dragDiv = document.getElementById("J_dragDiv");
        J_dragDiv.style.left = _event.clientX + "px";
        J_dragDiv.style.top = _event.clientY + "px";
    });

这里，元素的left和top值设成了鼠标相对屏幕的坐标，这就将元素的顶左上角定位到了鼠标的指针位置，就实现了简单的鼠标跟随效果。

利用这一点，只要监听鼠标的mousemove时间，让其执行这个函数即可。

先看一下demo：http://jser.io/onlinedemo/drag-and-drop/

以下是主要部分的源码：
  
    (function() {
        var dragDivObj = null;
        var innerPosLeft = 0,
            innerPosTop = 0;
        // 获取拖动元素的宽高
        var divWidth = 0,
            divHeight = 0;
        var followMouse = function(event) {
            var _event = event || window.event;
            var _target = _event.target || _event.srcElement;
            switch(_event.type) {
            case "mousedown":
                if(_target.className.indexOf("J_dragDiv") > -1) {
                    dragDivObj = _target;
                    // 将正在被拖动的节点移到所有兄弟节点的最后
                    dragDivObj.parentNode.appendChild(dragDivObj);
                    // 将正在被拖动的元素变半透明
                    dragDivObj.style.filter = "alpha(opacity=50)";
                    dragDivObj.style.opacity = "0.5";
                    // 获取拖动元素的宽高
                    divWidth = dragDivObj.scrollWidth;
                    divHeight = dragDivObj.scrollHeight;
                    // 元素内部的点击位置
                    innerPosTop = _event.clientY - _target.offsetTop;
                    innerPosLeft = _event.clientX - _target.offsetLeft;
                }
                break;
            case "mousemove":
                if(null !== dragDivObj) {
                    //清除选择
                    window.getSelection ? 
                        window.getSelection().removeAllRanges() 
                        : document.selection.empty();

                    // 元素的坐标
                    var posLeft = _event.clientX - innerPosLeft;
                    var posTop = _event.clientY - innerPosTop;

                    // 限定坐标不超出浏览器的边界
                    dragDivObj.style.left = Math.min(Math.max(posLeft, 0),
                        document.documentElement.clientWidth - divWidth) + "px";
                    dragDivObj.style.top = Math.min(Math.max(posTop, 0),
                        document.documentElement.clientHeight - divHeight) + "px";

                }
                break;
            case "mouseup":
                if(null !== dragDivObj) {
                    // 释放鼠标，恢复半透明元素
                    dragDivObj.style.filter = "alpha(opacity=100)";
                    dragDivObj.style.opacity = "1";
                    dragDivObj = null;
                }
                break;
            }
        }

        function init() {
            EventUtil.addEventHandler(document, "mousedown", followMouse);
            EventUtil.addEventHandler(document, "mousemove", followMouse);
            EventUtil.addEventHandler(document, "mouseup", followMouse);
        }
        init();
    })();
 

由于上面的跟随效果被拖动元素只有左上角会跟随鼠标指针，所以在计算元素坐标过程中，将鼠标点击在元素内部的坐标计算进去，这样拖动的过程中元素就不会跑到元素的左上角了。

首先得到点击在元素内部的坐标：

  
    innerPosTop = _event.clientY - _target.offsetTop;  
    innerPosLeft = _event.clientX - _target.offsetLeft;

将鼠标在屏幕上的坐标减去在元素内部的坐标值即为元素应在的坐标位置：
  
    var posLeft = _event.clientX - innerPosLeft;  
    var posTop = _event.clientY - innerPosTop;
 
一般的拖动都会跟随一个透明的效果，来使交互更好，所以在mousemove方法中让拖动元素变成半透明，鼠标释放以后在让其变回原样：

    dragDivObj.style.filter = "alpha(opacity=50)"; // IE  
    dragDivObj.style.opacity = "0.5"; // FF

这里对IE要做一下兼容处理。

如果想要变回原形，将样式的filter和opacity分别设置成100和1即可。

如果想让当前拖动的元素显示在所有元素的最上层，则将其移动到父节点的最后即可：

    dragDivObj.parentNode.appendChild(dragDivObj);

通常我们的拖动是不希望元素超出浏览器视口范围的，所以要在拖动的过程中对元素坐标做一下判断，让其不超出浏览器的范围，这样就既不会出现滚动条，也不会使元素移出浏览器的可视范围了。

    dragDivObj.style.left = Math.min(Math.max(posLeft, 0),  
        document.documentElement.clientWidth - divWidth) + "px";
    dragDivObj.style.top = Math.min(Math.max(posTop, 0),  
        document.documentElement.clientHeight - divHeight) + "px";

拖动的过程中，如果页面上有文字的话会被选中，所以要清除掉选中的范围：

    window.getSelection ?  
        window.getSelection().removeAllRanges() : document.selection.empty();

点击这里可以在线试一下：http://jser.io/onlinedemo/drag-and-drop/

点击这里可以下载demo源码：drag-and-drop.zip