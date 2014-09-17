---
layout: post
title:  "Nginx Wordpress 固定链接和自定义404页面"
date:   2013-03-20 01:57:00
categories: Wordpress 固定链接 Nginx
---
通常我们都将博客的文章链接设置成固定链接，使我们文章链接看起来更加的友好，也更加容易被搜索引擎收录。

上网搜索了好多重写规则，如下：

    location / {
        if (-f $request_filename/index.html){
            rewrite (.*) $1/index.html break;
        }
        if (-f $request_filename/index.php){
            rewrite (.*) $1/index.php;
        }
        if (!-f $request_filename){
            rewrite (.*) /index.php;
        }
    }

不过当想自己自定义404页面的时候，开启fastcgi_intercept_errors on;

但是由于上面规则的原因，无法跳转到所配置的404页面。

如此，可以使用wordpress官方的配置方法，如下是我的配置文件：

    server {
        listen  80;
        server_name  jser.io jser.io;
        root  /var/www;
        index  index.html index.htm index.php;

        location ~ .*\.(php|php5)?$ {
            # 如下使Nginx支持php
            fastcgi_pass 127.0.0.1:9000;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME /data/www$fastcgi_script_name;
            include fastcgi_params;
        }

        location / {
            try_files $uri $uri/ /index.php?$args;
        }

        if (!-e $request_filename) {
            rewrite ^/[_0-9a-zA-Z-]+(/wp-.*) $1 last;
            rewrite ^/[_0-9a-zA-Z-]+.*(/wp-admin/.*\.php)$ $1 last;
            rewrite ^/[_0-9a-zA-Z-]+(/.*\.php)$ $1 last;
        }
    }

至此，所有的wordpress正常运行。