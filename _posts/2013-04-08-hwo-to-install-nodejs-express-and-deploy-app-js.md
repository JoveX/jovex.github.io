---
layout: post
title:  "nodejs : express的安装和app.js配置"
date:   2013-04-08 01:18:00
categories: ExpressJS Node.js
---
MVC可谓无人不知，无人不晓。它不但可以增强团队的协作，还可以容易的进行产品的维护和升级。本文将对nodejs的一个MVC开发框架express.js做一下简单的介绍。

####express.js的安装

如果你已经安装了npm，那么事情将变得非常简单：

    npm install express -d

-d代表将安装express以及相关套件。这里还有一个-g，代表安装到NODE_PATH的lib里面。

如果没有npm可以去github上面下载一个express。

好了，下面创建一个express实例

    cd ~  
    express testobject  
    cd testobject  
    node app.js

这样就创建了一个名为testobject的express实例，并已经将其启动了，现在浏览器访问127.0.0.1:3000就可以看到效果了。这里的app.js是程序的主文件，下面讲解一下app.js里面的相关配置。

####模块的引用

    var express = require('express'),  
        routes = require('./routes'),
        user = require('./routes/user'),
        http = require('http'),
        path = require('path');
    var app = express();

require是nodejs提供的一个函数，可以用它来将有用的模块包含进来，从而调用其函数和变量。require默认会从NODE_PATH和当前js所在目录下的node_modules来调用模块，而且require也可以将自己写的模块包含进来。

####app.js的配置详细说明

express.js是从connect模块继承来的，所以也需要同时安装connect模块，不过之前安装express时，已经包含进来了的。

    app.set('port', process.env.PORT || 3000);

设置服务所开放的端口默认为3000，在后面启动服务的时候可以直接用get调用这个端口号。

    app.set('views', __dirname + '/views');

设置程序所用的views文件夹，即模板文件夹。&#95;&#95;dirname代表当前执行js所在的文件夹路径。另外，&#95;&#95;filename代表当前执行文件的文件名。

    app.set('view engine', 'jade');

设置express所用的模板引擎为jade，express支持多种模板引擎，常用的有，常用的有：

* haml的实现Haml
* haml.js接替者，同时也是Express的默认模板引擎Jade
* 嵌入JavaScript模板EJS
* 基于CoffeeScript的模板引擎CoffeeKup
* 的NodeJS版本jQuery模板引擎

可以根据自己的需要选择模板。

    app.use(express.favicon());
    app.use(express.logger('dev'));

设置程序的图标为express的图标，并将工作日志打印到后台显示。

    app.use(express.bodyParser());

bodyParser()是Connect的内建middleware，这样可以将用户提交过来的数据放到request.body中。

    app.use(express.methodOverride());

methodOverride()也是Connect的内建middleware，可以协助post处理和伪装其他http method，如put和delete。

    app.use(app.router);

router在express官方说这句是可有可无的，删掉以后还真可以正常运行，不过还是写上吧。

    app.use(express.static(path.join(__dirname, 'public')));

static()同样是Connect的内建middleware，是用来协助静态的request请求的，如css、js和img等静态文件，所以在static里面的，会全部作为静态形式返回给用户。

    app.use(express.errorHandler());

errorHandler()一样是Connect的内建middleware，用来处理异常的，这个一般在开发过程中会用到，上线以后将其去掉。

####路由和request的处理

    app.get('/', routes.index);

这里是直接走到首页模板，利用所调用的自建模块index来使用其中的方法和属性。在index中的内容如下：

    /*
     * GET home page.
     */

    exports.index = function(req, res) {  
        res.render('index', {
            title: 'Express'
        });
    };

这里是读取index模板，并传进去title为Express，这个title在模板中就可以直接调用了的。

index模板的内容如下：

    extends layout

    block content  
        h1= title
        p Welcome to #{title}

这个模板是继承自layout模板的，可以直接将此模板的内容到到layout模板的body里面。

layout模板如下：

    doctype 5  
    html  
        head
            title= title
            link(rel='stylesheet', href='/stylesheets/style.css')
        body
            block content

> 注意：模板代码的缩进，要么只使用tab，要么只使用空格，如果两个同时使用程序会抛错误。

如果想处理post请求，则可以利用app.post()。

    app.post('/add', function(req, res) {  
        res.render('add', {
            title: "Hello World",
            sum: req.body.a + req.body.b
        });
    });

前面提到了，post的数据可以放到request.body里面，这里利用了express.bodyParser()。

除了post和get之外，还有app.all()方法，意思是处理所有的请求。

####启动nodejs服务

    http.createServer(app).listen(app.get('port'), function() {  
        console.log('Express server listening on port ' + app.get('port'));
    });

OK，目前为止，所有的express配置都已经讲解完了，可以自己做一个简单的nodejs服务来供访问了。