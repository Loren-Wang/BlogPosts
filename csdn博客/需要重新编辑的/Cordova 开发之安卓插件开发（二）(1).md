前面的一篇文章讲了开发安卓的插件，但是是在已经添加好的平台中进行的开发，还不算是插件，仅仅只是相当于一个安卓程序，汗，这才是仅仅第一步啊，不过这个第一步也是得有点安卓基础的人才能搞得明白，其实是有点像是安卓的第三方插件开发了，哈哈哈！

话不多说，说点正经的，其实整个的插件制作最重要的其实就是一个文件**plugin.xml**文件，这个文件其实起着枢纽的作用，他会把插件里面的文件在你使用命令行build工程的时候将文件生成到指定的位置上，这个指定位置其实就是在我上一篇文章介绍插件开发的时候的文件目录！

加入几篇参考文章：
[ Cordova开发自定义插件（详细篇含jar包调用）](http://blog.csdn.net/lyc_xiaochao/article/details/52947575?locationNum=10&fps=1)
[cordova Plugin.xml 详解](http://www.jianshu.com/p/92dd69ae7d8f)

附上我的plugin.xml的源码（和上一篇文章的介绍的工程不是一个，大家不要介意，一开始是在人家弄好的插件上修改的，后续加上自己的理解慢慢修改的，其实就是一开始有问题没弄通，也忘了记下来了，汗）

```
<?xml version='1.0' encoding='utf-8'?>
<plugin id="lycPlugin" version="1.0.0" xmlns="http://apache.org/cordova/ns/plugins/1.0" xmlns:android="http://schemas.android.com/apk/res/android">
    <name>LycPlugin</name>

    <!--指js文件名，而这个文件会自动以`<script`>标签的形式添加到Cordova项目的起始页。
    通过在*js-module*中列出插件，可以减少开发者的工作。*clobbers*元素说明了
    *module.exports*自动添加到*window*对象，让插件方法能够在窗口级别使用。-->
    <!--文件中js-module元素定义了js的名字，它将在应用开始时自动加载。
    它定义了向Cordova公开的js接口。clobbers元素指明了js对象赋值给加载的js对象。
    本例中，LycPlugin插件通过一个lyc.toast对象向Cordova应用公开。-->
    <js-module name="LycPlugin" src="www/LycPlugin.js">
        <clobbers target="lyc.toast" />
    </js-module>

    <!--它定义了某个移动平台专用的设置，包括了相关native代码的设置。可以有一个或多个平台。-->
    <!--android的配置-->
    <platform name="android">

        <!--config-file元素定义了在插件安装过程中的改动。在例子中，一个叫LycToast
        的特性添加到Android项目的config.xml文件中，指向Java类com.lyc.toast.LycPlugin-->
        <config-file parent="/*" target="res/xml/config.xml">
            <feature name="LycToast">
                <param name="android-package" value="com.lyc.toast.LycPlugin" />
            </feature>
        </config-file>

        <!--android权限，本例没有用到下权限，仅示例-->
        <config-file parent="/*" target="AndroidManifest.xml">
          <!--连接网络权限，用于执行云端语音能力 -->
          <uses-permission android:name="android.permission.INTERNET"/>
          <!--获取手机录音机使用权限，听写、识别、语义理解需要用到此权限 -->
          <uses-permission android:name="android.permission.RECORD_AUDIO"/>
          <!--读取网络信息状态 -->
          <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
          <!--获取当前wifi状态 -->
          <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
          <!--允许程序改变网络连接状态 -->
          <uses-permission android:name="android.permission.CHANGE_NETWORK_STATE"/>
          <!--读取手机信息权限 -->
          <uses-permission android:name="android.permission.READ_PHONE_STATE"/>
          <!--读取联系人权限，上传联系人需要用到此权限 -->
          <uses-permission android:name="android.permission.READ_CONTACTS"/>
          <!--外存储写权限，构建语法需要用到此权限 -->
          <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
          <!--外存储读权限，构建语法需要用到此权限 -->
          <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
          <!--配置权限，用来记录应用配置信息 -->
          <uses-permission android:name="android.permission.WRITE_SETTINGS"/>
          <!--手机定位信息，用来为语义等功能提供定位，提供更精准的服务-->
          <!--定位信息是敏感信息，可通过Setting.setLocationEnable(false)关闭定位请求 -->
          <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
          <!--如需使用人脸识别，还要添加：摄相头权限，拍照需要用到 -->
          <uses-permission android:name="android.permission.CAMERA" />

          <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
          <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
          <uses-permission android:name="android.permission.INTERNET" />
          <uses-permission android:name="android.permission.READ_PHONE_STATE" />
          <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
          <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
          <uses-permission android:name="android.permission.WAKE_LOCK" />
          <uses-permission android:name="com.android.launcher.permission.READ_SETTINGS" />
          <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />
          <uses-permission android:name="android.permission.CHANGE_WIFI_MULTICAST_STATE" />
          <uses-permission android:name="android.permission.INTERACT_ACROSS_USERS_FULL" />
        </config-file>


       <config-file target="AndroidManifest.xml" parent="/manifest/application">  
                 <activity
            android:configChanges="orientation|keyboardHidden|keyboard|screenSize|locale" 
            android:label="@string/activity_name"
            android:launchMode="singleTop"
            android:name=".MainActivity"
            android:theme="@android:style/Theme.DeviceDefault.NoActionBar" 
            android:windowSoftInputMode="adjustResize">
            <intent-filter android:label="@string/launcher_name">
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity android:name="com.lyc.toast.PlayerActivity"
            android:configChanges="orientation|keyboard|keyboardHidden|screenSize|screenLayout|uiMode"
            android:theme="@android:style/Theme.Light.NoTitleBar"></activity>
        </config-file>  

        <hook type="after_prepare" src="www/android_socket.js" />




        <!--source-file元素指出了一个或多个Android native源代码文件，当插件安装时由CLI安装。
        下面的例子指示plugman或CLI复制LycPlugin.java文件到Cordova项目
         Android平台文件夹的*src/com/lyc/toast/LycPlugin文件夹中。jar包放到libs下-->
        <source-file src="src/android/LogUtils.java" target-dir="src/com/lyc/toast" />
        <source-file src="src/android/LycPlugin.java" target-dir="src/com/lyc/toast" />
        <source-file src="src/android/XunFeiVoiceDictationUtils.java" target-dir="src/com/lyc/toast" />
        <source-file src="src/android/XunFeiVoiceSynthesisUtils.java" target-dir="src/com/lyc/toast" />
        <source-file src="src/android/IatSettings.java" target-dir="src/com/lyc/toast" />
        <source-file src="src/android/JsonParser.java" target-dir="src/com/lyc/toast" />
        <source-file src="src/android/MyApplication.java" target-dir="src/com/lyc/toast" />
        <source-file src="src/android/SettingTextWatcher.java" target-dir="src/com/lyc/toast" />
        <source-file src="src/android/TtsSettings.java" target-dir="src/com/lyc/toast" />
        <source-file src="src/android/PlayerActivity.java" target-dir="src/com/lyc/toast" />
        <source-file src="src/android/MainActivity.java" target-dir="src/com/lyc/toast" />
        <source-file src="src/android/R.java" target-dir="src/com/lyc/toast" />
        


        <source-file src="src/android/values/my_strings.xml" target-dir="res/values" />
        <source-file src="src/android/values/values.xml" target-dir="res/values" />
        <source-file src="src/android/xml/iat_setting.xml" target-dir="res/xml" />
        <source-file src="src/android/xml/tts_setting.xml" target-dir="res/xml" />

        <source-file src="src/android/layout/activity_player.xml" target-dir="res/layout" />
        <source-file src="src/android/layout/layout_ad.xml" target-dir="res/layout" />
        <source-file src="src/android/layout/layout_mid.xml" target-dir="res/layout" />
        <source-file src="src/android/layout/vidqitem.xml" target-dir="res/layout" />
        <source-file src="src/android/drawable/nonedrawable.xml" target-dir="res/drawable" />
        <source-file src="src/android/drawable/play_title_bkg.xml" target-dir="res/drawable" />
        <source-file src="src/android/drawable/player_control_bg.xml" target-dir="res/drawable" />
        <source-file src="src/android/drawable/player_loading.xml" target-dir="res/drawable" />
        <source-file src="src/android/drawable/player_loading_normal.xml" target-dir="res/drawable" />
        <source-file src="src/android/drawable/popwinselector.xml" target-dir="res/drawable" />
        <source-file src="src/android/drawable/quality_bkg.xml" target-dir="res/drawable" />
        <source-file src="src/android/drawable/vidqbg.xml" target-dir="res/drawable" />
        <source-file src="src/android/drawable/vidqtxt.xml" target-dir="res/drawable" />
        <source-file src="src/android/drawable/yp_progress_holo_light.xml" target-dir="res/drawable" />
        <source-file src="src/android/drawable/yp_progressbarstyle.xml" target-dir="res/drawable" />
        <source-file src="src/android/drawable/yp_progressthumbstyle.xml" target-dir="res/drawable" />
        <source-file src="src/android/drawable/yw_1222_0335_mwua.jpg" target-dir="res/drawable" />

        <source-file src="src/android/drawable-xhdpi-v4/ad_bg_back.9.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/ad_bg_full.9.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/ad_icon_arrow.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/ad_icon_volume.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/ad_icon_volume_off.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/full_icon_back.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/icon_pause.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/icon_fullscreen.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/icon_pause_noband.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/icon_play.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/icon_play_noband.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/icon_scrubbarslider.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/icon_smallscreen.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/loading.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/loading_normal.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/player_logo_tudou.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/player_logo_youku.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/plugin_ad_more_youku.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/seekbar_bkg.9.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/seekbar_front_progress.9.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/seekbar_second_progress.9.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/vertical_icon_back.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/vertical_logo.png" target-dir="res/drawable-xhdpi-v4" />
        <source-file src="src/android/drawable-xhdpi-v4/vertical_logo_tudou.png" target-dir="res/drawable-xhdpi-v4" />

        <source-file src="src/android/drawable-xxhdpi-v4/ad_bg_back.9.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/ad_bg_full.9.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/ad_close.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/ad_icon_arrow.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/ad_icon_fullscreen.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/ad_icon_out.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/ad_icon_volume.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/ad_icon_volume_off.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/full_icon_back.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/icon_fullscreen.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/icon_pause.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/icon_play.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/icon_scrubbarslider.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/icon_smallscreen.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/loading_normal.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/player_logo_youku.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/seekbar_bkg.9.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/seekbar_front_progress.9.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/seekbar_second_progress.9.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/vertical_icon_back.png" target-dir="res/drawable-xxhdpi-v4" />
        <source-file src="src/android/drawable-xxhdpi-v4/vertical_logo.png" target-dir="res/drawable-xxhdpi-v4" />






         <!--讯飞语音文件 -->
        <source-file src="src/android/libs/Msc.jar" target-dir="libs" />
        <source-file src="src/android/libs/Sunflower.jar" target-dir="libs" />
        <source-file src="src/android/libs/armeabi/libmsc.so" target-dir="libs/armeabi" />
        <source-file src="src/android/libs/armeabi-v7a/libmsc.so" target-dir="libs/armeabi-v7a" />
        <source-file src="src/android/libs/mips/libmsc.so" target-dir="libs/mips" />
        <source-file src="src/android/libs/mips64/libmsc.so" target-dir="libs/mips64" />
        <source-file src="src/android/libs/x86/libmsc.so" target-dir="libs/x86" />
        <source-file src="src/android/libs/x86_64/libmsc.so" target-dir="libs/x86_64" />

        <source-file src="src/android/assets/iflytek/recognize.xml" target-dir="assets/iflytek" />
        <source-file src="src/android/assets/iflytek/voice_bg.9.png" target-dir="assets/iflytek" />
        <source-file src="src/android/assets/iflytek/voice_empty.png" target-dir="assets/iflytek" />
        <source-file src="src/android/assets/iflytek/voice_full.png" target-dir="assets/iflytek" />
        <source-file src="src/android/assets/iflytek/waiting.png" target-dir="assets/iflytek" />
        <source-file src="src/android/assets/iflytek/warning.png" target-dir="assets/iflytek" />



        <!--优酷视频文件 -->

        <source-file src="src/android/libs/armeabi-v7a/libcrypto.so" target-dir="libs/armeabi-v7a/" />
        <source-file src="src/android/libs/armeabi-v7a/libMMANDKSignature.so" target-dir="libs/armeabi-v7a/" />
        <source-file src="src/android/libs/armeabi-v7a/libnetcache.so" target-dir="libs/armeabi-v7a/" />
        <source-file src="src/android/libs/armeabi-v7a/libsgavmp.so" target-dir="libs/armeabi-v7a/" />
        <source-file src="src/android/libs/armeabi-v7a/libsgmain.so" target-dir="libs/armeabi-v7a/" />
        <source-file src="src/android/libs/armeabi-v7a/libsgsecuritybody.so" target-dir="libs/armeabi-v7a/" />
        <source-file src="src/android/libs/armeabi-v7a/libssl.so" target-dir="libs/armeabi-v7a/" />
        <source-file src="src/android/libs/armeabi-v7a/libuffmpeg.so" target-dir="libs/armeabi-v7a/" />
        <source-file src="src/android/libs/armeabi-v7a/libuplayer23.so" target-dir="libs/armeabi-v7a/" />
        <source-file src="src/android/libs/mma_sdk.jar" target-dir="libs" />
        <source-file src="src/android/libs/utdid4all-1.1.5.5.jar" target-dir="libs" />
        <framework src="com.alibaba:fastjson:1.1.56.android"/>
        <framework src="com.nostra13.universalimageloader:universal-image-loader:1.9.5"/>
        <framework src="src/android/MyPlugin.gradle" custom="true" type="gradleReference"/>
        <resource-file src="src/android/libs/YoukuPlayerOpenSDK-release.aar" target="libs/YoukuPlayerOpenSDK-release.aar"/>

   
      

    </platform>
</plugin>
```
**ps：**各标签含义我这里就不多说了，代码中都有，下面我会说一下各文件的导入，但是在导入各个文件之前有一些东西是要先说的，首先说的是插件下各文件所放置的位置。
    ![这里写图片描述](http://img.blog.csdn.net/20170904215215252?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
  ①.是所有的资源文件所在的路径，最好是按照开发安卓插件的时候各个资源文件的文件夹层级关系放置，方便管理，也方便在将文件路径添加到plugin配置文件中；
  ②.是所有的开发文件放置的位置，这些文件最好直接将你已经开发好的文件复制过来，因为在最后build的时候系统不会去自动寻找文件的，而在配置plugin文件的时候这些文件路径一定要与各文件的import关键字导入的文件路径相同，同时R文件也要变成固定变量导入；
  ③.该文件是插件的入口，是继承了CordovaPlugin类的一个子类，同时重写了execute方法，该方法是插件的初始入口，各参数代表的含义这里就不多说了，大家可以自行百度；
  ④.该文件是application初始化文件，有一些sdk等第三方库需要在application中初始化参数配置的东西在这个里面；
  ⑤.R文件，记录着所有的资源id，当开发文件中需要用到资源id的时候需要在这里面找，相当于一个映射表，在原生开发中一般使用系统自动生成的R文件，但是在这种第三方插件当中想要使用资源文件则必须使用写死固定的R文件，否则会出现找不到R文件的问题；
  ⑥.gradle文件，有一些特殊的文件的导入不光需要在plugin中配置，还需要在gradle文件中也做特殊的配置，例如.aar文件。


接下来是各文件功能的导入方法：
---------------
   

 - **首先是用户权限的导入**
        安卓的权限问题只要做过安卓开发的都知道，有一些特殊的权限需要在开发文件中请求，那些我就不在多说了，但是所有的权限也是都要在**AndroidManifest.xml**文件中声明的，导入的方式如下：
```
 <config-file parent="/*" target="AndroidManifest.xml">
      <uses-permission android:name="android.permission.INTERNET"/>
 </config-file>
```
 - **接下来是用户的activity的注册**
       activity的注册也是在**AndroidManifest.xml**文件当中，但是不同的是他是要在application标签的下面，所以格式同权限的导入不太一样，需要修改父节点，但是有一点和权限的导入一样的是你可以直接把开发中的activity标签的整个内容直接复制过来，如下：
   

```
 <config-file target="AndroidManifest.xml" parent="/manifest/application"> 
  
      <activity 
            android:name=".MainActivity"
            android:configChanges="orientation|keyboardHidden|keyboard|screenSize|locale" 
            android:label="@string/activity_name"
            android:launchMode="singleTop"
            android:theme="@android:style/Theme.DeviceDefault.NoActionBar" 
            android:windowSoftInputMode="adjustResize">
            <intent-filter android:label="@string/launcher_name">
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        
        <activity android:name="com.lyc.toast.PlayerActivity"      android:configChanges="orientation|keyboard|keyboardHidden|screenSize|screenLayout|uiMode"
            android:theme="@android:style/Theme.Light.NoTitleBar"></activity>
        </config-file>  

```

 - **接下来是修改AndroidManifest.xml文件的application标签了**
    因为在插件的制作过程当中需要用到application开发文件，所以在正常的开发中需要用药application标签的name属性，但是又因为在plugin的标签当中没有一个可以直接修改application标签的，所以只能使用hook标签来间接的修改，如下：
```
<hook type="after_prepare" src="www/android_socket.js" />
```

 这里的src路径下的www文件指的是这个文件：
 ![这里写图片描述](http://img.blog.csdn.net/20170904222646733?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 
**文件内容如下：**

```
#!/usr/bin/env node

module.exports = function(context) {

  var fs = context.requireCordovaModule('fs'),
    path = context.requireCordovaModule('path');

  var platformRoot = path.join(context.opts.projectRoot, 'platforms/android');


  var manifestFile = path.join(platformRoot, 'AndroidManifest.xml');

  if (fs.existsSync(manifestFile)) {

    fs.readFile(manifestFile, 'utf8', function (err,data) {
      if (err) {
        throw new Error('Unable to find AndroidManifest.xml: ' + err);
      }

      var appClass = 'com.lyc.toast.MyApplication';

      if (data.indexOf(appClass) == -1) {

        var result = data.replace(/<application/g, '<application android:name="' + appClass + '"');

        fs.writeFile(manifestFile, result, 'utf8', function (err) {
          if (err) throw new Error('Unable to write into AndroidManifest.xml: ' + err);
        })
      }
    });
  }


};
```

 - **接下来是插件资源文件的导入**
    插件的资源文件的导入时很方便的，直接使用“source-file”标签导入就好了，标签内的**src**属性是在插件的资源文件路径，而**target-dir**属性值得是最后将插件添加进入平台的时候资源文件存放的路径，详细的介绍请参考最上方的链接。
   
   
 -  **网络资源的导入**
    网络资源的导入只要有过使用as开发的人看一下代码就明白了。

```
        <framework src="com.alibaba:fastjson:1.1.56.android"/>
        <framework src="com.nostra13.universalimageloader:universal-image-loader:1.9.5"/>
```

 - **gradle文件的导入**

```
        <framework src="src/android/MyPlugin.gradle" custom="true" type="gradleReference"/>
```

 - **.aar文件的导入**
       .aar文件的导入需要gradle文件的配合才能正确的导入到项目工程当中

```
       <resource-file src="src/android/libs/YoukuPlayerOpenSDK-release.aar" target="libs/YoukuPlayerOpenSDK-release.aar"/>
```

 - **最后是调用方法的编写**
    调用方法的编写使用的是js方法编写的，存放的路径如下图LycPlugin.js文件的位置：
     ![这里写图片描述](http://img.blog.csdn.net/20170904222646733?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

    方法调用内容：
```
var exec = require('cordova/exec');

var lycFunc = function(){};
// arg1：成功回调
// arg2：失败回调
// arg3：将要调用类配置的标识
// arg4：调用的原生方法名
// arg5：参数，json格式
lycFunc.prototype.xunFeiVoiceDictationStart = function(success,error) {
     exec(
            success, // success callback function
            error, // error callback function
            'LycToast', // mapped to our native Java class called "CalendarPlugin"
            'xunFeiVoiceDictationStart', // with this action name
            [{                  // and this array of custom arguments to create our entry
               "title": "init",
            }]
        );
     };

lycFunc.prototype.xunFeiVoiceDictationStop = function(success,error) {
        exec(
            success, // success callback function
            error, // error callback function
            'LycToast', // mapped to our native Java class called "CalendarPlugin"
            'xunFeiVoiceDictationStop', // with this action name
            [{                  // and this array of custom arguments to create our entry
               "title": "init",
            }]
        );
     };  

lycFunc.prototype.xunFeiVoiceDictationCancel =  function(success,error) {
        exec(
            success, // success callback function
            error, // error callback function
            'LycToast', // mapped to our native Java class called "CalendarPlugin"
            'xunFeiVoiceDictationCancel', // with this action name
            [{                  // and this array of custom arguments to create our entry
               "title": "init",
            }]
        );
     };  

lycFunc.prototype.xunFeiVoiceSynthesisStart =  function(synthesisText,success,error) {
        exec(
            success, // success callback function
            error, // error callback function
            'LycToast', // mapped to our native Java class called "CalendarPlugin"
            'xunFeiVoiceSynthesisStart', // with this action name
            [{                  // and this array of custom arguments to create our entry
               "title": synthesisText,
            }]
        );
     };  

lycFunc.prototype.xunFeiVoiceSynthesisCancel =  function(success,error) {
        exec(
            success, // success callback function
            error, // error callback function
            'LycToast', // mapped to our native Java class called "CalendarPlugin"
            'xunFeiVoiceSynthesisCancel', // with this action name
            [{                  // and this array of custom arguments to create our entry
               "title": "init",
            }]
        );
     };

lycFunc.prototype.youKuPlayerVideo =  function(vid,success,error,error) {
        exec(
            success, // success callback function
            error, // error callback function
            'LycToast', // mapped to our native Java class called "CalendarPlugin"
            'youKuPlayerVideo', // with this action name
            [{                  // and this array of custom arguments to create our entry
               "vid": vid,
            }]
        );
     }; 


var showt = new lycFunc();
module.exports = showt;
```