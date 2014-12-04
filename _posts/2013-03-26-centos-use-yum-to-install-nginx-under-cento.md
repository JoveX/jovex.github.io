---
layout: post
title:  "CentOS利用yum安装Nginx"
date:   2013-03-26 10:55:00
categories: CentOS Nginx yum
---
因为CentOS没有默认的Nginx的源，所以需要启用REHL的附件包。

    rpm -Uvh http://nginx.org/packages/centos/6/noarch/RPMS/nginx-release-centos-6-0.el6.ngx.noarch.rpm

现在就可以用yum来安装Nginx了

    yum -y install nginx

Nginx官网的下载地址为：

http://nginx.org/en/download.html

可以从上面找到相应版本的安装包。