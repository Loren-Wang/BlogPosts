cordova其实就是一种移动web的框架，它的前身就是PhoneGap。后来PhoneGap捐献给Apache后，抽离出核心代码，就改名为cordova。Cordova支持如下移动操作系统：iOS, Android,ubuntu phone os, Blackberry, Windows Phone, Palm WebOS, Bada 和 Symbian。所以说应用还是很广泛的。

但是cordova的安装确实一件很麻烦的事情，所以下面介绍一下如何搭建cordova环境平台。

1. 下载node.js
------------

官网地址是https://nodejs.org/en/，下载安装之后还要进行环境配置 （部分电脑不用配置会被自动添加进去）
![这里写图片描述](http://img.blog.csdn.net/20160824092613560)

在命令行下进行测试：node -v 
![这里写图片描述](http://img.blog.csdn.net/20160824092855766)

2.安装npm
-----

其实在安装Node.js的时候，已经顺带安装好了npm。 
npm其实是node.js的包管理工具。我们在node.js上开发时，会用到很多别人写的JavaScript代码。如果我们要使用别人写的某个包，就可以直接通过npm安装使用，不用管代码存在哪，应该从哪下载。这是因为大家都把自己开发的模块打包后放到npm官网上。 
![这里写图片描述](http://img.blog.csdn.net/20160824094233224)

3.JDK安装和SDK
-----------

通常的Java和android开发人员，jdk就基本都安装和配置好的了，而sdk的话，我比较建议是直接安装android studio，因为这里面有附带安装的，而我因为一早就安装好了android studio
系统变量值设置（系统变量里没有的变量名称就新增变量）
JAVA_HOME --------D:\Java\jdk1.8.0_91
PATH--------%JAVA_HOME%\bin
CLASSPATH--------  	.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar
ANDROID_HOME--------D:\AndroidSdk

4.安装cordova
-----------
国内使用命令行安装cordova基本上很难安装上，毕竟因为限制被挡住了，所以只能通过其他的途径来解决了，依次执行下面的代码就可以安装了。
```
1)  npm  install express   
2)  npm install -g ionic 
3)  npm install -g cordova 
```
安装成功之后， ![这里写图片描述](http://img.blog.csdn.net/20160824101923490)

5.使用cordova命令一直报错（如果没报错的话可以忽略掉这一步了）
-----------------------------------
在安装路径下（这里的安装路径是c:\nodejs）添加两个文件夹node_cache和node_global文件
![这里写图片描述](http://images2015.cnblogs.com/blog/965234/201705/965234-20170511144613238-1138467385.png)








ps：参考文章链接
      [Cordova环境安装配置 - lihuanxing的博客 - CSDN博客](http://blog.csdn.net/lihuanxing/article/details/52297815)
      [ npm install -g ionic cordova 安装失败的解决方案](http://blog.csdn.net/capmiachael/article/details/52368916)
      [node遇到的一些坑，npm无反应，cordova安装以后显示不是内部或外部命令](http://www.cnblogs.com/lhyhappy65/p/6840924.html)