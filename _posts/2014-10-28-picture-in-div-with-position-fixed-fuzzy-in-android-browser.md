---
layout: post
title:  "position:fixed在Android原生浏览器造成图片模糊"
date:   2014-10-28 15:48:12
categories: 
---
最近遇到一个问题，当给一个div加上position:fixed时，在某些Android原生浏览器div里面的背景图片显示模糊，而同部手机的其他浏览器就不会有此问题。即使换成2倍大小的图片也会展示模糊。  
```html
<div style="position:fixed;">
    <img src="logo.png">
</div>
```
但是如果把div的position:fixed去掉图片就又光鲜亮丽了，目测可能是Android的原生浏览器bug。  
给图片添加以下css样式使该图片使用硬件加速就可以解决此问题：  
```css
img {
    -webkit-transform: scale(1) rotate(0) translate3d(0,0,0);
    transform: scale(1) rotate(0) translate3d(0,0,0);
}
```

后话：在Android原生浏览器上经常会出现各种问题，可以尝试使用硬件加速试试看。但是过度使用会给浏览器造成巨大的压力，最好只在关键地方使用硬件加速，不要过度使用。