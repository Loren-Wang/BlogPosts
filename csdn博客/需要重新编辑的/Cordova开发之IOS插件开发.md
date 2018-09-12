由于本人是安卓开发出身，并没有开发过ios，所以这篇文章只是帮朋友写的，大家如果感觉有问题的话那就只当做一个参考就好了。

**在正式描述之前有几点是要提前说明的：**
①.首先本篇文章适合有一定ios基础的朋友们；
②.本篇文章不会讲如何将第三方sdk导入到ios项目中并开发和第三方项目相关的功能；
③.本篇文章主要讲的是**plugin.xml**的配置，即一些特殊文件的导入方法在xml中是怎么设置的；
④.ios的开发环境安装以及Cordova工程的创建等请自行百度，也可以参考前面的几篇文章，之所以把我自己写的几篇文章添加进来是因为在ios方面的插件开发与安卓的部分逻辑是相似的，可以在百度的同时如果不明白的在参考一下我的文章；
[ Cordova 开发之环境搭建](http://blog.csdn.net/cmwly/article/details/77510820)
[ Cordova 开发之安卓插件开发（一）](http://blog.csdn.net/cmwly/article/details/77247840)
[ Cordova 开发之安卓插件开发（二）](http://blog.csdn.net/cmwly/article/details/77843852)
[Cordova开发自定义插件（详细篇含jar包调用）](http://blog.csdn.net/lyc_xiaochao/article/details/52947575?locationNum=10&fps=1) 
[cordova Plugin.xml 详解](http://www.jianshu.com/p/92dd69ae7d8f)

**首先先上代码：**

```
<?xml version="1.0" encoding="UTF-8" ?>
<plugin xmlns="http://phonegap.com/ns/plugins/1.0"
    id="com.will.cordovaPlugin"
    version="1.0.0">
    <engines>
        <engine name="cordova" version=">=3.3.0" />
    </engines>
    
    <name>XFPlugin</name>
    <description>测试插件</description>
    
    <js-module src="www/testPlugin.js" name="willModel">
        <clobbers target="testModel" />
    </js-module>
    
    <platform name="ios">
        <header-file src="src/ios/Definition.h"  />
        <header-file src="src/ios/IATConfig.h" />
        <source-file src="src/ios/IATConfig.m" />
        
        <header-file src="src/ios/ISRDataHelper.h" />
        <source-file src="src/ios/ISRDataHelper.m" />
        
        <header-file src="src/ios/PlayerViewController.h" />
        <source-file src="src/ios/PlayerViewController.m" />
        
        <header-file src="src/ios/TTSConfig.h" />
        <source-file src="src/ios/TTSConfig.m"/>
        
        <header-file src="src/ios/XFPlugin.h" />
        <source-file src="src/ios/XFPlugin.m"/>
        
        <header-file src="src/ios/XFTool.h" />
        <source-file src="src/ios/XFTool.m" />
        
        <!--讯飞依赖-->
        <framework src="src/ios/iflyMSC.framework" custom="true" />
        
        <!--优酷云依赖-->
        <framework src="src/ios/library/BCUserTrack/UTMini.framework" parent="YouTuEngineMediaPlayer/library/BCUserTrack/" custom = "true"/>
        <framework src="src/ios/library/SecurityGuardSDK/SecurityGuardSDK.framework" parent="YouTuEngineMediaPlayer/library/SecurityGuardSDK/"  custom = "true"/>
        <framework src="src/ios/library/SGAVMP/SGAVMP.framework" parent="YouTuEngineMediaPlayer/library/SGAVMP/"  custom = "true"/>
        <framework src="src/ios/library/SGMain/SGMain.framework" parent="YouTuEngineMediaPlayer/library/SGMain/"  custom = "true"/>
        <framework src="src/ios/library/SGSecurityBody/SGSecurityBody.framework" parent="YouTuEngineMediaPlayer/library/SGSecurityBody/"  custom = "true" />
        <framework src="src/ios/library/UTDID/UTDID.framework" parent="YouTuEngineMediaPlayer/library/UTDID/"  custom = "true" />
        
        <source-file src="src/ios/YouTuEngineMediaPlayer/cloud.bundle"  target-dir="YouTuEngineMediaPlayer/"/>
        <source-file src="src/ios/YouTuEngineMediaPlayer/libYouTuMediaPlayerEngineYouku.a" target-dir="YouTuEngineMediaPlayer/" framework = "true"/>
        <source-file src="src/ios/YouTuEngineMediaPlayer/yw_1222_0335_mwua.jpg" target-dir="YouTuEngineMediaPlayer/"/>
        
        <source-file src="src/ios/library/Reachability/LICENCE.txt" target-dir="YouTuEngineMediaPlayer/library/Reachability/"/>
        <source-file src="src/ios/library/Reachability/README.md"  target-dir="YouTuEngineMediaPlayer/library/Reachability/"/>
        <source-file src="src/ios/library/Reachability/Reachability.h"  target-dir="YouTuEngineMediaPlayer/library/Reachability/" />
        <source-file src="src/ios/library/Reachability/Reachability.m" target-dir="YouTuEngineMediaPlayer/library/Reachability/" />
        
        <source-file src="src/ios/YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/YoukuMediaPlayer.h" target-dir="YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/"/>
        <source-file src="src/ios/YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/YTDownloadDefine.h" target-dir="YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/"/>
        <source-file src="src/ios/YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/YTDownloadManager.h" target-dir="YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/"/>
        <source-file src="src/ios/YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/YTDownloadTaskModel.h" target-dir="YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/"/>
        <source-file src="src/ios/YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/YTEngineOpenViewManager.h" target-dir="YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/"/>
        <source-file src="src/ios/YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/YTLocalMedia.h" target-dir="YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/"/>
        <source-file src="src/ios/YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/YTSequence.h" target-dir="YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/"/>
        <source-file src="src/ios/YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/YYMediaPlayer.h" target-dir="YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/"/>
        <source-file src="src/ios/YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/YYMediaPlayerBackgroundModeManager.h" target-dir="YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/"/>
        <source-file src="src/ios/YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/YYMediaPlayerDefines.h" target-dir="YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/"/>
        <source-file src="src/ios/YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/YYMediaPlayerEvents.h" target-dir="YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/"/>
        <source-file src="src/ios/YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/YYMediaPlayerException.h" target-dir="YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/"/>
        <source-file src="src/ios/YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/YYMediaPlayerHistory.h" target-dir="YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/"/>
        <source-file src="src/ios/YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/YYMediaPlayerItem.h" target-dir="YouTuEngineMediaPlayer/YouTuMediaPlayerEngineYoukuHeaders/"/>
        
        
        <!--系统依赖-->
        <framework src="libz.tbd" />
        <framework src="libresolv.9.tbd" />
        <framework src="libiconv.2.tbd" />
        <framework src="libxml2.2.tbd" />
        <framework src="libbz2.1.0.tbd" />
        <framework src="libc++.1.tbd" />
        <framework src="libsqlite3.tbd" />
        <framework src="libicucore.tbd" />
        <framework src="libc++.tbd" />
        <framework src="libz.tbd" />
        
        <framework src="VideoToolbox.framework" />
        <framework src="CoreMedia.framework" />
        <framework src="OpenGLES.framework" />
        <framework src="CoreText.framework" />
        <framework src="AdSupport.framework" />
        <framework src="MediaPlayer.framework" />
        <framework src="EventKit.framework" />
        <framework src="MessageUI.framework" />
        <framework src="Social.framework" />
        <framework src="MobileCoreServices.framework" />
        <framework src="CoreMotion.framework" />
        <framework src="ModeliO.framework"  />
        
        <framework src="CoreGraphics.framework" />
        <framework src="QuartzCore.framework" />
        <framework src="AddressBook.framework" />
        <framework src="Contacts.framework" />
        <framework src="CoreLocation.framework" />
        <framework src="UIKit.framework" />
        <framework src="AudioToolbox.framework" />
        <framework src="CoreTelephony.framework" />
        <framework src="Foundation.framework" />
        <framework src="SystemConfiguration.framework" />
        <framework src="AVFoundation.framework" />
        
        
        
        <config-file target="config.xml" parent="/*">
            
            <feature name="XFPlugin">
                <param name="ios-package" value="XFPlugin" />
            </feature>
            
        </config-file>
    </platform>
</plugin>

```

 - **.h文件的引入**
    使用header-file标签来引入
```
   <header-file src="src/ios/Definition.h"  />
```
 - **.m文件的引入**
    使用source-file标签引入
```
   <source-file src="src/ios/IATConfig.m" />
```
 - **第三方sdk的引入**
     首先说明的是 framework后缀的文件的引入，至于其属性src以及parent的含义可以查看上面的参考文章，但是这里有一点很重要的就是**custom**的属性，这个属性是针对第三方sdk来说是必须的；

```
 <framework src="src/ios/library/BCUserTrack/UTMini.framework" parent="YouTuEngineMediaPlayer/library/BCUserTrack/" custom = "true"/>
```

 - **.a文件的导入**
    这是一个特殊的第三方库文件，至于他的导入也很特殊，除了正常导入需要的source-file标签外，还需要该标签的一个特殊的属性，即**framework="true"**这个属性，添加这个属性是为了是.a文件的导入可以向framework文件一样的方式导入；
```
<source-file src="src/ios/YouTuEngineMediaPlayer/libYouTuMediaPlayerEngineYouku.a" target-dir="YouTuEngineMediaPlayer/" framework = "true"/>
```
 - **第三方剩余文件的导入**
    第三方库剩余的一些文件的导入其实没什么好说的了，直接使用普通的source-file标签正常导入即可；
    
 - **系统依赖库的导入**
    其实这也没什么好说的，直接使用framework标签正常导入就好；
```
     <!--系统依赖-->
     <framework src="libz.tbd" />
     <framework src="VideoToolbox.framework" />
```

 - **最终要的一步**
     接下来就剩最重要的一步了，没有这一步的话基本上是无法调用插件的，那就是配置**config.xml**文件，需要向这个文件中添加一个**feature**标签的，如下：
 

```
       <config-file target="config.xml" parent="/*">
            <feature name="XFPlugin">
                <param name="ios-package" value="XFPlugin" />
            </feature>
       </config-file>
```