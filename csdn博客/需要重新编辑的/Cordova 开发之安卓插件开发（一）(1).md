准备工作
----

  想要开发Cordova插件首先要安装环境[Cordova 开发之环境搭建](http://blog.csdn.net/cmwly/article/details/77510820)，其次还要准备一些必要的软件（具体的下载地址可以自行百度，我后续也可能会把我自己经常用的发上来）：

 - Android Studio 开发工具
 - Android Sdk 开发工具包
 - Android Ndk 开发工具包（这个最好加上，有些东西是需要ndk支持的）


开始开发
----

 1. 创建Cordova工程 
      使用Cmd命令行在当前的文件夹下创建Cordova工程：cordova create CordovaAndroidPluginsDemo com.cordovaandroidplugins.cordova CordovaAndroidPluginsDemo 
 2. 添加平台（windows下） 
      其实就是需要打包的平台，你要那个平台的插件就需要添加哪个平台，使用命令：C:\Users\username\Desktop\CordovaProject>cordova platform add android添加相应平台的代码。
 3. 打开工程
     想要制作插件就要先打开一个空白的Cordova的工程，将整个安卓平台的工程跑通之后才能开始插件的制作。平台添加后使用Android Studio 的open方式 打开项目，而且选择打开的一定要是platforms–>android–>builder.gradle文件,打开界面如下图：
     ![这里写图片描述](http://img.blog.csdn.net/20170816150353874?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 4. 添加MyPlugin.Java文件
     在如下图的地方创建MyPlugin.Java类继承CordovaPlugin.java且重写excute()方法： 
     ![这里写图片描述](http://img.blog.csdn.net/20170816150720550?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
     
     ![这里写图片描述](http://img.blog.csdn.net/20170816151105751?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 5. 设置Plugin文件
     创建完成文件之后在工程的res/xml/config.xml中配置对应的plugin: 
     ![这里写图片描述](http://img.blog.csdn.net/20170816151423998?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 6. 添加js文件
     .在工程的assests/www/plugins或者assests/www/js 下新建文件carrier.js文件。 
     ![这里写图片描述](http://img.blog.csdn.net/20170816151705244?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 7. 修改index.html文件 
     ![这里写图片描述](http://img.blog.csdn.net/20170816152113974?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 8. 导入第三方sdk 
     按照需要导入的第三方的sdk的要求导入相关文件以及添加相应代码，该工程中使用的是讯飞和优酷视频，所以将jar以及aar文件放入到lib文件夹中同时使用add library 功能添加给工程，同时将build.gradle文件中找到dependencies{}加入如下代码（根据第三方sdk而定，如果缺失则查第三方文档） 
     ![这里写图片描述](http://img.blog.csdn.net/20170816152542498?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 9. 使用第三方sdk根据第三方的说明使用相关代码调用第三方sdk，本文中有使用则是导入以下代码 
     ![这里写图片描述](http://img.blog.csdn.net/20170816152758899?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 10. 最后使用Android Studio 编译运行即可
