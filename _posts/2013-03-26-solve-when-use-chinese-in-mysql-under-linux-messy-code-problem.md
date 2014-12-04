---
layout: post
title:  "MySQL在Linux下中文乱码问题解决"
date:   2013-03-26 10:18:00
categories: Linux MySQL 中文乱码
---
为了学习，自己搭建了一个CentOS的服务器，利用yum源安装的MySQL，可是发现写入中文的时候数据库乱码。

逐步排查发现，从数据提交到Servlet直到底层写入数据库之前都是正常的，可偏偏写入数据库以后变成了问号。

OK，发现问题，初步认定是数据库编码造成的。

在数据库执行：

    show variables like 'char%';

发现character_set_database和character_set_server项都是latin1，问题出现在这里，数据库的编码默认不是utf8编码，所以导致中文变成了问号。

解决方法如下：

如果是MySQL5.0或MySQL5.1

在其中的[mysqld]和[client]下分别添加：

    default-character-set=utf8

如果是MySQL5.5

在[mysqld]下添加：

    character-set-server=utf8

在[client]下添加：

    default-character-set=utf8

此时重启MySQL服务，再提交数据一切正常啦～