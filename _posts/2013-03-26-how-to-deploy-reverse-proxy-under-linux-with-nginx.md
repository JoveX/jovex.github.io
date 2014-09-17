---
layout: post
title:  "Linux下Nginx反向代理设置"
date:   2013-03-26 10:44:00
categories: Linux Nginx 反向代理
---
Nginx不但是大家常用的web服务器，同时也是一个高性能的反向代理服务器，下面说说反向代理的配置。

由于服务器不仅仅运行php，还有部分jsp的程序，所以需要利用反向代理来指定到本地相应的端口。

以下是对服务器下一个jsp网站代理的例子：

  
    server {  
        listen 80;
        server_name portal.jser.io;
        server_tokens off; # 隐藏nginx版本号
        location / {
            proxy_pass http://localhost:8080/;
            proxy_redirect off;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }

如果portal.jser.io访问此服务器的话，直接从服务器内部转到Tomcat的8080端口，实现了反向代理。

可以利用此方法对一台服务器配置多个站点，通过不同的域名来指定到不同的目录或者端口。