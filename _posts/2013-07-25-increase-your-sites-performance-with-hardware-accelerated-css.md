---
layout: post
title:  "启用CSS的硬件加速来提高网站的性能（译）"
date:   2013-07-25 14:30:25
categories: Html/Css HTML5 前端优化
---
原来一直以为只要用了CSS3的animation，浏览器就会自动让GPU去处理，可是了解到，这个也需要开启用GPU来硬件渲染，否则将会像普通的动画一样对待，并不会启动硬件加速。

上网找了一篇文章感觉不错，翻译过来分享一下，原地址为：

http://blog.teamtreehouse.com/increase-your-sites-performance-with-hardware-accelerated-css

![The Ghost Logo](/userfiles/images/hardware-accelerated-css.jpg)

#####译文：

你知道吗，我们可以在浏览器中让CSS开启GPU（Graphics Processing Unit）硬件加速，来提升网站的性能。

现在大多数电脑都有支持硬件加速的显卡。鉴于此，我们可以充分利用GPU的硬件来使我们的网站比在CPU上运行的更加流畅。

在本篇文章中，我们涵盖了在PC浏览器和移动浏览器的CSS3的硬件加速用法。

在桌面和移动浏览器开启CSS硬件加速

有没有想过为什么CSS3的动画在一些浏览器中会如此的流畅？

CSS的animations，transforms和transitions​并不会自动开启GPU加速，取而代之的是利用缓慢的浏览器软件来渲染执行。那么，我们怎么样才能让浏览器切换到GPU模式呢？很多浏览器提供的一些触发的CSS规则。

目前，像Chrome，FireFox，Safari，IE9+和最新版本的Opera都支持硬件的加速；仅仅需要一个特定的CSS规则，相应的DOM就会开启GPU加速。用CSS最显著的规则就是对元素开启3D变换。

比如：

    .cube {
        -webkit-transform: translate3d(250px,250px,250px)
        rotate3d(250px,250px,250px,-120deg)
        scale3d(0.5, 0.5, 0.5);
    }

有些时候，我们有些并不需要3D的图形变换可还依然要开启GPU的加速。那么用如下这几个CSS属性就可以让浏览器触发GPU的硬件加速了。

虽然我们对该DOM元素并不需要3D变换，可是依然能让其触发3D渲染。我们可以利用transform: translateZ(0);来触发无论桌面还是移动设备的GPU加速。这看起来是触发GPU加速最有效的方式（包括所有的浏览器）：

    .cube {
        -webkit-transform: translateZ(0);
        -moz-transform: translateZ(0);
        -ms-transform: translateZ(0);
        -o-transform: translateZ(0);
        transform: translateZ(0);
        /* Other transform properties here */
    }

在Chrome和Safari浏览器中，CSS的transforms方法或animations方法可能会造成浏览器的闪烁。下面的方法可以修复这个问题：

    .cube {
        -webkit-backface-visibility: hidden;
        -moz-backface-visibility: hidden;
        -ms-backface-visibility: hidden;
        backface-visibility: hidden;

        -webkit-perspective: 1000;
        -moz-perspective: 1000;
        -ms-perspective: 1000;
        perspective: 1000;
        /* Other transform properties here */
    }

在Webkit内核的桌面和移动设备浏览器中，另一个有效的方法是translate3d：

    .cube {
        -webkit-transform: translate3d(0, 0, 0);
        -moz-transform: translate3d(0, 0, 0);
        -ms-transform: translate3d(0, 0, 0);
        transform: translate3d(0, 0, 0);
        /* Other transform properties here */
    }

移动设备的本地应用也会很好的利用GPU的加速，这就是为什么他们相对WebApp渲染的比较好的原因。在移动设备上使用硬件的加速是非常有用的，因为它可以有效的减少移动设备资源的占用。

#####总结

我们所介绍的方法应该只是在绑定了动画的元素上使用，它虽然比2D变换上性能更优越，可是在每个元素上都加上硬件加速显然是不明智的。

小心使用这些方法，并且保证性能确实有所提高再去使用这些方法。使用GPU加速可能会导致严重的系统性能问题，因为它提高了系统内存的使用量，并且也对移动设备电池也有一定影响。

你在项目中使用过这些方法么？如果有，请分享给大家。