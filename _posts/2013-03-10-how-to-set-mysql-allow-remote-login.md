---
layout: post
title:  "MySQL允许远程登录"
date:   2013-03-10 02:49:00
categories: MySQL 远程登录
---
通常我们在开发中用的都是root账户连接到localhost或者127.0.0.1，但是如果想远程发布到测试机，连接的时候则提示

    1045 access denied for user 'root'@'10.88.0.14' using password yes

这是因为没有对远程用户开启远程访问权限。

####解决方法：

#####方法1：
使用grant授权用户，允许root用户在所有IP段连接MySQL

    GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'adminpassword' WITH GRANT OPTION;

用户名和密码可以根据自己的需要设定。%代表所有的IP段。

如果到此还不能远程登录，则再执行如下指令，刷新MySQL所有权限相关表。

    flush privileges;


 

如果只是想让某一个IP访问此MySQL服务，则可以将%换成指定IP，如：

    GRANT ALL PRIVILEGES ON *.* TO 'root'@'10.88.0.14' IDENTIFIED BY 'adminpassword' WITH GRANT OPTION;

这里同上，如果还不能远程登录，则刷新一下MySQL权限相关表。

    flush privileges;

 

#####方法2：
可以直接登录到远程服务器，利用localhost访问MySQL，直接修改表，方法如下：

在远程服务器登录MySQL

    [root@speed01 ~]# mysql -uroot -ppassword

先将数据库切换到mysql

    mysql> use mysql;
    Database changed

    /* 先查看一下user表，这个表是存放数据库所有授权用户信息的表 */
    mysql> select host, user from user;
    +————————————-+——–+
    | host                    | user |
    +————————————-+——–+ 
    | 127.0.0.1               | root |
    | localhost               |      |
    | localhost               | root |
    +————————————-+——–+
    3 rows in set (0.00 sec)

发现root用户只允许localhost和127.0.0.1登录 直接修改user表

    mysql> update user set host = '%' where user = 'root';
    
此时再查看user表，root相关的用户，已经将访问的IP段换成了%，即允许所有的IP段访问了。

    mysql> select host, user from user;
    +————————————-+——–+
    | host                    | user | 
    +————————————-+——–+
    | %                       | root | 
    | localhost               |      | 
    | %                       | root | 
    +————————————-+——–+ 
    3 rows in set (0.00 sec)

再刷新一下MySQL系统权限相关表，注意这里的flush privileges;是非常有必要写的，只有执行了这句话，授权才能生效。

    mysql> flush privileges;