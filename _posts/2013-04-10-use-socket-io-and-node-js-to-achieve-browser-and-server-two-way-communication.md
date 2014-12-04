---
layout: post
title:  "使用socket.io和node.js实现浏览器和客户端的双向通信"
date:   2013-04-10 05:10:00
categories: Node.js socket.io
---
WebSocket protocol 是HTML5一种新的协议(protocol)。它是实现了浏览器与服务器全双工通信(full-duplex)。

在 WebSocket API，浏览器和服务器只需要要做一个握手的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。

####socket.io的安装

socket.io的安装同express一样可以非常容易的安装：

    npm install socket.io

如果没有安装npm的可以直接去github下载。

####socket.io的使用

其实socket.io的使用非常容易，简单来说只多了两个方法--on和emit。

#####client端

首先需要引入socket.io准备的js：
  
    <script src="/socket.io/socket.io.js"></script>

再在client连接server端，并能够实现收发数据：

    var socket = io.connect('http://localhost';);  
    socket.on('news', function(data) {  
        console.log(data);
        socket.emit('my other event', {
            my: 'data'
        });
    });

connect就是连接到服务器的地址，对服务器创建一个连接。

on方法会接收到服务器发来的消息，并执行其后的function，里面的data就是后端传过来的数据。emit即向服务器端发送消息，第二个对象就是向后端发送的数据。

接收和发送消息，都是通过on和emit方法的第一个字符串决定执行哪个on函数的。

#####server端

服务器端跟客户端代码类似，一样是用on和emit来收发消息的。

那么先开启一个服务，来读取客户端的html文件，然后返回给客户端：

    var app = require('http').createServer(handler),  
        io = require('socket.io').listen(app),
        fs = require('fs')

        app.listen(80);

    function handler(req, res) {  
        fs.readFile(__dirname + '/index.html', function(err, data) {
            if(err) {
                res.writeHead(500);
                return res.end('Error loading index.html');
            }

            res.writeHead(200);
            res.end(data);
        });
    }

    io.sockets.on('connection', function(socket) {  
        socket.emit('news', {
            hello: 'world'
        });
        socket.on('my other event', function(data) {
            console.log(data);
        });
    });

这样，浏览器访问[http://localhost](http://localhost)的时候就会执行其中的收发消息的函数，可以从浏览器和控制台看到消息的日志了。

server端向客户端发送消息也有几个不同的方式：

只发送消息给客户端：

    socket.emit('news', {  
        hello: 'world'
    });

因为每个客户端与服务器连接都会创建一个socket，可以将这个socket根据用户名放到一个对象中，这样就可以根据用户名来实现只有单一用户能接收到消息，即可实现私聊的功能。

广播信息给除当前用户之外的用户：

    socket.broadcast.emit('user connected');

广播给全部客户端：

    io.sockets.emit('all users');

到此就已经全部结束了，想做到收发数据其实非常的简单，意犹未尽是吧？那就自己真正去体验一下，用小东西组成大的项目吧。