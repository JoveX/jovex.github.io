---
layout: post
title:  "各浏览器对同一域名的最大连接数"
date:   2013-07-22 13:14:55
categories: 浏览器并发
---
现在几乎所有的互联网公司都不止有一个域名，主站、JavaScript、Css都分别存放于不同的域名下，就是为了减少浏览器对同一域名的并发数，加快加载速度。

各个浏览器对同一域名的并发数如下：

<table border="1" cellpadding="4" cellspacing="0">
    <thead>
        <tr>
            <th scope="col">
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">Browser</span></span>
            </th>
            <th scope="col">
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">HTTP/1.1</span></span>
            </th>
            <th scope="col">
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">HTTP/1.0</span></span>
            </th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">IE 6,7</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">2</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">4</span></span>
            </td>
        </tr>
        <tr>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">IE 8</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">6</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">6</span></span>
            </td>
        </tr>
        <tr>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">Firefox 2</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">2</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">8</span></span>
            </td>
        </tr>
        <tr>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">Firefox 3</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">6</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">6</span></span>
            </td>
        </tr>
        <tr>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">Safari 3,4</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">4</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">4</span></span>
            </td>
        </tr>
        <tr>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">Chrome 1,2</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">6</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">?</span></span>
            </td>
        </tr>
        <tr>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">Chrome 3</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">4</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">4</span></span>
            </td>
        </tr>
        <tr>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">Chrome 4+</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">6</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">?</span></span>
            </td>
        </tr>
        <tr>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">Chrome 11+</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">6</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">?</span></span>
            </td>
        </tr>
        <tr>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">iPhone 2</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">4</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">?</span></span>
            </td>
        </tr>
        <tr>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">iPhone 3</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">6</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">?</span></span>
            </td>
        </tr>
        <tr>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">iPhone 4</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">4</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">?</span></span>
            </td>
        </tr>
        <tr>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">Opera 9.63,10.00alpha</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">4</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">4</span></span>
            </td>
        </tr>
        <tr>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">Opera 10.51+</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">8</span></span>
            </td>
            <td>
                <span style="font-size:14px;"><span style="font-family:courier new,courier,monospace;">?</span></span>
            </td>
        </tr>
    </tbody>
</table>

如果想深入学习，可以转到

http://www.stevesouders.com/blog/2008/03/20/roundup-on-parallel-connections/