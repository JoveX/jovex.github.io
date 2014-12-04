---
layout: post
title:  "安装node依赖提示没有.npm目录的访问权限"
date:   2014-06-28 15:43:38
categories: Node.js JavaScript NPM
---
以前在全局下用NPM安装过Express的项目生成工具：  

    npm install -g express-generator


最近在Mac上通过express指令生成Express项目的时候发现进入到项目后，无法通过`npm install`安装项目依赖。  

    express ExpressProjectName


提示没有`.npm`目录的访问权限。  

    npm ERR! Error: EACCES, open '/Users/xyq/.npm/debug/0.7.4/package/package.json'
    npm ERR!  { [Error: EACCES, open '/Users/xyq/.npm/debug/0.7.4/package/package.json']
    npm ERR!   errno: 3,
    npm ERR!   code: 'EACCES',
    npm ERR!   path: '/Users/xyq/.npm/debug/0.7.4/package/package.json',
    npm ERR!   parent: 'sohunews-node' }
    npm ERR!
    npm ERR! Please try running this command again as root/Administrator.

    npm ERR! System Darwin 13.2.0
    npm ERR! command "node" "/usr/local/bin/npm" "install"
    npm ERR! cwd /Users/xyq/project/sohu/sohunews-node
    npm ERR! node -v v0.10.29
    npm ERR! npm -v 1.4.14
    npm ERR! path /Users/xyq/.npm/debug/0.7.4/package/package.json
    npm ERR! code EACCES
    npm ERR! errno 3
    npm ERR! stack Error: EACCES, open '/Users/xyq/.npm/debug/0.7.4/package/package.json'
    npm ERR!
    npm ERR! Additional logging details can be found in:
    npm ERR!     /Users/xyq/project/sohu/sohunews-node/npm-debug.log
    npm ERR! not ok code 0


想来可能是因为执行`.npm`目录是在root用户下创建的，而我当前执行`npm install`的用户是非root用户。  
SO，把根目录下的`.npm`目录权限更改为当前用户的权限就OK了。

    sudo chown -R yourusername ~/.npm


如果还是不行，那么清空一下tmp目录就应该好用了。

    rmdir ~/tmp


现在再执行`npm install`应该就可以正常安装项目依赖了。