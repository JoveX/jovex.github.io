---
layout: post
title:  "程序在Linux服务器上MySQL报表不存在错误"
date:   2013-03-26 11:09:30
categories: Linux MySQL
---
前些日子将所有Windows服务器上的网站都移植到了Linux上面，可是一系列问题就出现了。

首先就是程序报java.sql.SQLException: Table 'xxx.TB_XXX' doesn't exist，可是数据库的表是存在的。

查询后发现是因为SQL语句中用的是大写，可是由于Linux识别大小写，所以报错。

解决方法如下:

在MySQL配置文件my.cnf中[mysqld]下面加上

    # 1表示不区分大小写
    # 0表示区分大小写
    lower\_case\_table\_names=1

重启MySQL服务，一切正常～