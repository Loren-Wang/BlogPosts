因为自己写了一些框架，但是有不想以包的形式导入到项目当中，但是又想用自己写的框架，所以，就想到了发布到maven中央库，下面就开始介绍怎么发布了！
## **准备工作** ##

 1. GPG签名，工具下载地址：[https://www.gnupg.org/download/index.html](https://www.gnupg.org/download/index.html)，按照下图1-1中的内容下载相应的平台的安装包即可！
![1-1](http://img.blog.csdn.net/20171106152911836?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 2. 开源库（snapshot或release）账户，注册地址：[https://issues.sonatype.org/](https://issues.sonatype.org/)


## **开始进入发布流程** ##






## **遇到的问题** ##

 1. Could not resolve all dependencies for configuration ':classpath'与
      Failed to resolve: com.android.support:appcompat-v7:26.1.0
       解决：[https://stackoverflow.com/questions/45188703/could-not-find-com-android-tools-build-gradle3-0-0-alpha7](https://stackoverflow.com/questions/45188703/could-not-find-com-android-tools-build-gradle3-0-0-alpha7)
       其实就是把google()替换成 maven { url 'https://maven.google.com' }
       
 2. gpg: conflicting commands maven
       解决：其实就是命令内容不完全，有些事双杠的在使用命令的时候写成了单杠，
                 例如：`gpg -keyserver hkp://pool.sks-keyservers.net -send-keys F86B173C`会报错
                 但是：`gpg --keyserver hkp://pool.sks-keyservers.net --send-keys F86B173C`就没有报错







参考链接：[https://my.oschina.net/looly/blog/270767](https://my.oschina.net/looly/blog/270767)
参考链接：[http://www.cnblogs.com/tiantianbyconan/p/4388175.html](http://www.cnblogs.com/tiantianbyconan/p/4388175.html)
参考链接：[http://blog.csdn.net/xxl6097/article/details/50325309/](http://blog.csdn.net/xxl6097/article/details/50325309/)
参考链接：[https://github.com/xxl6097/gradle-mvn-push](https://github.com/xxl6097/gradle-mvn-push)