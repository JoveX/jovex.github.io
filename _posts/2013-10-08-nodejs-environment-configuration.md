---
layout: post
title:  "Node.js环境配置"
date:   2013-10-08 12:50:08
categories: CentOS Linux Node.js JavaScript
---
最近在CentOS上开发Node.js的应用程序，把开发环境配置过程记录一下。

安装环境

先升级您的系统内核：

    yum update -y


准备一下安装环境：

    yum install gcc-c++ openssl-devel curl git-core build-essential libssl-dev  
    yum groupinstall "Development Tools"


确认一下python版本：

    python -V


我的版本是Python 2.6.6，需要升级到2.7才行。

不过在升级之前，需要安装Python的bz2库，否则nodejs会无法编译，抛出如下错误：

    ImportError: No module named bz2。


需要先安装bz2库才行：

    yum install bzip2*


此时再编译安装Python：

    wget -c http://www.python.org/ftp/python/2.7.5/Python-2.7.5.tar.bz2  
    tar jxvf Python-2.7.5.tar.bz2  
    cd Python-2.7.5  
    ./configure
    make && make install


安装完新版本的Python，需要更下一下系统的指向：

    mv /usr/bin/python /usr/bin/python.bak  
    ln -s /usr/local/bin/python2.7 /usr/bin/python


此时系统yum将无法使用了，需要将yum中Python版本做如下修改：

    vi /usr/bin/yum


将文本编辑器中第一行：

    #!/usr/bin/python


修改为

    #!/usr/bin/python2.6


即你旧的Python版本。

保存并退出。

安装nodejs

    wget -c http://nodejs.org/dist/v0.10.20/node-v0.10.20.tar.gz  
    tar zxvf node-v0.10.20.tar.gz  
    cd node-v0.10.20  
    ./configure –prefix=/usr/local/nodejs –openssl-libpath=/usr/local/ssl/lib/ –openssl-includes=/usr/local/ssl/include/
    make && make install


现在为止nodejs已经全部安装完成了，可是由于之前的项目用到了canvas，使用npm install canvas可能还会出现错误，例如 no cairo package ...，可以通过如下方式解决：

    yum install cairo cairo-devel cairomm-devel libjpeg-turbo-devel pango pango-devel pangomm pangomm-devel giflib-devel

