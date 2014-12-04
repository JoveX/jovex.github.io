---
layout: post
title:  "CSS3 background-size控制背景图尺寸"
date:   2013-06-04 13:38:28
categories: Html/Css HTML5
---
background-size这个属性其实是在做响应式布局的时候用到了的。

当时目的是要让背景div按照比例缩放来适应各种浏览器的尺寸，可是背景图是一张大图，这样就需要背景图片铺满整个div。

下面简单说一下background-size这个属性的用法吧。

#####语法

background-size ：[ &lt;length&gt; | &lt;percentage&gt; | auto ]{1,2} | cover | contain

#####兼容性

<table style="border: 1px solid rgb(204, 204, 204); border-collapse: collapse; text-align: center; margin-top: 0.54em; font-family: 微软雅黑, 华文新魏; color: rgb(102, 102, 102); line-height: 18px;">
    <tbody>
        <tr>
        </tr>
    </tbody>
    <thead>
        <tr>
            <th class="type" style=" padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204); background-color: rgb(243, 243, 243); background-position: initial initial; background-repeat: initial initial;">
                类型
            </th>
            <th class="type_ie" style=" padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204); background-color: rgb(243, 243, 243); background-position: initial initial; background-repeat: initial initial;">
                <img alt="IE" src="/userfiles/images/compatible_ie.gif" style="border: 0px; display: block; margin: 0.45em auto 0px; height: 30px; width: 31px;" />Internet Explorer
            </th>
            <th class="type_firefox" style=" padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204); background-color: rgb(243, 243, 243); background-position: initial initial; background-repeat: initial initial;">
                <img alt="Firefox" src="/userfiles/images/compatible_firefox.gif" style="border: 0px; display: block; margin: 0.45em auto 0px; height: 30px; width: 31px;" />Firefox
            </th>
            <th class="type_chrome" style=" padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204); background-color: rgb(243, 243, 243); background-position: initial initial; background-repeat: initial initial;">
                <img alt="Chrome" src="/userfiles/images/compatible_chrome.gif" style="border: 0px; display: block; margin: 0.45em auto 0px; height: 30px; width: 31px;" />Chrome
            </th>
            <th class="type_opera" style=" padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204); background-color: rgb(243, 243, 243); background-position: initial initial; background-repeat: initial initial;">
                <img alt="Opera" src="/userfiles/images/compatible_opera.gif" style="border: 0px; display: block; margin: 0.45em auto 0px; height: 30px; width: 28px;" />Opera
            </th>
            <th class="type_safari" style=" padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204); background-color: rgb(243, 243, 243); background-position: initial initial; background-repeat: initial initial;">
                <img alt="Safari" src="/userfiles/images/compatible_safari.gif" style="border: 0px; display: block; margin: 0.45em auto 0px; height: 30px; width: 28px;" />Safari
            </th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="version" rowspan="4" style="text-align: left; padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204); font-weight: bold;">
                版本
            </td>
            <td class="support no" style="text-align: left; color: rgb(153, 153, 153); padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                <span style="margin-right: 4px;">(&times;)</span>IE6
            </td>
            <td class="support no" style="text-align: left; color: rgb(153, 153, 153); padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                <span style="margin-right: 4px;">(&times;)</span>Firefox 2.0
            </td>
            <td class="support yes" style="text-align: left; color: rgb(0, 136, 0); padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                <span style="margin-right: 4px;">(&radic;)</span>Chrome 1.0.x
            </td>
            <td class="support yes" style="text-align: left; color: rgb(0, 136, 0); padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                <span style="margin-right: 4px;">(&radic;)</span>Opera 9.63
            </td>
            <td class="support yes" style="text-align: left; color: rgb(0, 136, 0); padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                <span style="margin-right: 4px;">(&radic;)</span>Safari 3.1
            </td>
        </tr>
        <tr>
            <td class="support no" style="text-align: left; color: rgb(153, 153, 153); padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                <span style="margin-right: 4px;">(&times;)</span>IE7
            </td>
            <td class="support no" style="text-align: left; color: rgb(153, 153, 153); padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                <span style="margin-right: 4px;">(&times;)</span>Firefox 3.0
            </td>
            <td class="support yes" style="text-align: left; color: rgb(0, 136, 0); padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                <span style="margin-right: 4px;">(&radic;)</span>Chrome 2.0.x
            </td>
            <td style="text-align: left; padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                &nbsp;
            </td>
            <td class="support yes" style="text-align: left; color: rgb(0, 136, 0); padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                <span style="margin-right: 4px;">(&radic;)</span>Safari 4
            </td>
        </tr>
        <tr>
            <td class="support no" style="text-align: left; color: rgb(153, 153, 153); padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                <span style="margin-right: 4px;">(&times;)</span>IE8
            </td>
            <td class="support no" style="text-align: left; color: rgb(153, 153, 153); padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                <span style="margin-right: 4px;">(&times;)</span>Firefox 3.5
            </td>
            <td style="text-align: left; padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                &nbsp;
            </td>
            <td style="text-align: left; padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                &nbsp;
            </td>
            <td style="text-align: left; padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                &nbsp;
            </td>
        </tr>
        <tr>
            <td class="support yes" style="text-align: left; color: rgb(0, 136, 0); padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                <span style="margin-right: 4px;">(&radic;)</span>IE9
            </td>
            <td class="support yes" style="text-align: left; color: rgb(0, 136, 0); padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                <span style="margin-right: 4px;">(&radic;)</span>Firefox 3.6
            </td>
            <td style="text-align: left; padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                &nbsp;
            </td>
            <td style="text-align: left; padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                &nbsp;
            </td>
            <td style="text-align: left; padding: 0.2em 0.46em; border: 1px solid rgb(204, 204, 204);">
                &nbsp;
            </td>
        </tr>
    </tbody>
</table>

#####代码与实例

数值表示背景图尺寸

    #test-background-size-one {
        background-size: 300px 80px;
    }
 
百分比表示背景尺寸

    #test-background-size-two {
        background-size: 25% 75%;
    }
 
百分比数值混合使用表示背景尺寸

    #test-background-size-three {
        background-size: 200px 75%;
    }
 
等比扩展图片来填满元素

    #test-background-size-four {
        background-size: cover;
    }
 
等比缩小图片来适应元素的尺寸

    #test-background-size-five {
        background-size: contain;
    }
 
以图片自身大小来填充元素

    #test-background-size-six {
        background-size: auto;
    }
 
>用于设置背景图片的大小，有2个可选值，第1个值用于指定背景图的width，第2个值用于指定背景图的height，如果只指定1个值得，则第2个值默认为auto