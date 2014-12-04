---
layout: post
title:  "利用css3实现元素淡入"
date:   2014-06-27 10:15:06
categories: Html/Css css3
---
现在很有web页面都需要实现淡入淡出的效果，那么现在可以使用CSS3动画属性@keyframes可实现这一特效。  

首先定义淡入动画代码：  
```
@keyframes fade-in {
    0% {opacity: 0;}/*初始状态 透明度为0*/
    50% {opacity: 0;}/*过渡状态 透明度为0*/
    100% {opacity: 1;}/*结束状态 透明度为1*/
}  
@-webkit-keyframes fade-in {/*针对webkit内核*/
    0% {opacity: 0;}
    50% {opacity: 0;}
    100% {opacity: 1;}
}
```
在需要有淡化效果的元素上加入如下属性：  
```
#wrapper {
    animation-name: fade-in; /*动画名称*/
    animation-duration: 3s; /*动画持续时间*/
    animation-iteration-count: 1; /*动画次数*/
    animation-delay: 0s; /*延迟时间*/

    -webkit-animation-name: fade-in; /*动画名称*/
    -webkit-animation-duration: 3s; /*动画持续时间*/
    -webkit-animation-iteration-count: 1; /*动画次数*/
    -webkit-animation-delay: 0s; /*延迟时间*/
}
```
实际效果可以看本博客。
