---
layout: post
title:  "更改ejs模板后缀.ejs为.html"
date:   2014-07-07 16:51:47
categories: Express Node.js JavaScript EJS
---
现在很多项目中，html模板不仅仅只在后端或前端渲染，在有些项目中需要同时在前端或后端渲染的。  
为了避免某些麻烦，则可以将ejs模板后缀改为.html，保证一套模板可以同时在前端和后端使用。  
```javascript
//注册ejs模板为html页。简单的讲，就是原来以.ejs为后缀的模板页，现在的后缀名可以是.html了
app.engine('.html', require('ejs').__express);

//设置视图模板的默认后缀名为.html,避免了每次res.Render("xx.html")的尴尬
app.set('view engine', 'html');
```