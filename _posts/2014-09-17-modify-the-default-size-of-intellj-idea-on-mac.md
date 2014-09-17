---
layout: post
title:  "修改Intellj idea的默认内存设置"
date:   2014-09-17 23:31:05
categories: Mac IntelljIdea Java
---
今天准备在Mac上配置Java环境，IDE使用的是Intellj idea。  
当用maven编译项目的时候总提示系统资源不足，就是内存不足的意思。  
但是看看自己电脑的系统内存占用还是绿色的，完全没达到瓶颈。  
就考虑到是不是系统默认分配给Intellj idea的内存不够大造成的。  
查了下相关配置，可以按照如下步骤进行配置：  

 * 1、在Finder中找到`/Applications/Intellij Idea 13.app`
 * 2、选中`Intellij Idea 13.app`
 * 3、右键菜单中，选择“显示包内容”
 * 4、在`bin`目录下找到`idea.vmoptions`文件
 * 5、用文本编辑软件打开，我用的是`Sublime`
 * 6、修改`-Xmx750m`中的数值为你想要的内存值，推荐1024以上，我改成了2048

至此大功告成！  

其他系统也差不多，主要是找到`idea.vmoptions`这个文件。  
