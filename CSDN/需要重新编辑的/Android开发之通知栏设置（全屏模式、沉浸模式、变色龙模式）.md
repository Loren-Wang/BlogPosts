**基础概念：**

       1.全屏状态 ：默认不显示状态栏，下拉才有状态栏，并且会在几秒之后自动消失 
       2.沉浸模式 ：状态栏仍然一直存在，只不过背景颜色设为透明之后，布局向上填充了 。这种情况下如果在根布局中加入Android:fitsSystemWindows=”true”这个配置，布局就不会向上填充，而是把布局的背景色填充到状态栏背景区域。 
       3.变色龙模式 ：状态栏仍然一直存在，只不过背景颜色改变了，这种情况状态栏只是颜色改变了，并不会被布局填充。 

1、实现全屏显示效果
--------
       可以通过两种方式来设置全屏显示效果。是一种在代码中设置，第二种在manifest配置文件来设置。
      
 **1）代码中设置**
   
```
        //设置无标题  
        requestWindowFeature(Window.FEATURE_NO_TITLE);  
        //设置全屏  
        getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,   
                WindowManager.LayoutParams.FLAG_FULLSCREEN);  
        //必须在setContentView（）之前设置全屏
        setContentView(R.layout.main);  
```
**2）在manifest配置文件中设置**

```
 <activity android:name=".login.LoginActivity"   
                  android:theme="@android:style/android.NoTitleBar.Fullscreen"  
                  android:label="@string/app_name">  
            <intent-filter>  
                <action android:name="android.intent.action.MAIN" />  
                <category android:name="android.intent.category.LAUNCHER" />  
            </intent-filter>  
        </activity>  
```

2.实现沉浸模式
--------

**使用代码设置（在activity的onCreate方法中）**

```
        //透明状态栏
        getWindow().addFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS);
        //透明导航栏
        getWindow().addFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_NAVIGATION);
```
使用沉浸式会出现一个问题，具体解决：[http://blog.csdn.net/cmwly/article/details/71746210](http://blog.csdn.net/cmwly/article/details/71746210)

3.实现变色龙模式
---------
        还没有找到实现方法，不过按照我的理解其实就是沉浸式通知栏加上Android:fitsSystemWindows=”true”这个属性


4.设置状态栏透明度以及颜色
--------------