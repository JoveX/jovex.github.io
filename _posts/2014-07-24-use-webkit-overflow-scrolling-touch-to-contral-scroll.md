---
layout: post
title:  "css控制局部滚动达到弹性效果"
date:   2014-07-24 16:50:01
categories: Html/Css
---
开发移动设备上应用程序，经常会用到`overflow: scroll;`，达到局部滚动的效果。  
但是使用后发现，设置了这个属性之后，虽然可以局部滚动，手指离开屏幕移动就会立即停止，没有正常Safari滚动页面时候的那种弹性缓动效果。  
在css加上如下属性即可解决这个问题：  
```css
-webkit-overflow-scrolling: touch;
```