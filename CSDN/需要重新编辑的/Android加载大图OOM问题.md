    最近做了一个项目，花费了不少的时间，最主要的问题就是出现在了OOM内存溢出上，而且还主要在图片读取展示上最严重，真心很郁闷，最早开始的时候使用的是Volley加载的图片，不行，还是会出现溢出的问题，当然我加载的图片也有点多，总的原图大小算下来得将近40兆，所以说加载的时候很容易崩溃，尤其是加载多个界面之后，后来使用的就是一个专门的图片加载框架Picasso，当然同样的加载框架还有fresco、glide、ImageLoader等。
    在使用Picasso的时候我也出现了一个问题，那就是我无法清除缓存，我当时就很无语，执行了清除缓存的方法，但是内存占用率就是下不去，没办法找吧，最后各种方法都试了，还是下不去。唉，好没办法啊，干脆，把最消耗内存资源的Activity放到一个单独的进程当中，界面看完了就在finish()方法或者onDestory()方法当中将进程杀死就OK了，事实上这么做还真有效，但是这么做我知道有一点不好，不利于安全，但是我实在是没办法啊，如果大家有什么好的方法可以告诉我一下，还有如果这么做还有什么弊端也可以告诉小弟，小弟不胜感激！哈哈，闲话不多说了，现在开始说说过程：
    首先：
       先到配置文件“AndroidManifest.xml”当中找到将你要加入到另外一个进程当中的Activity，在这个Activity的属性当中增加一个属性：
  

```
     “android:process=":picture"”
```
      这个属性会在程序启动这个Activity的时候跳转到另外一个进程当中进行启动，不会影响主进程的运行，他与主进程之间的传值可以像是Activity页面跳转那样传值，两者之间没有什么区别！

    其次：
       在这个Activity的finish()或者onDestory()方法当中加入以下代码杀死进程
  

```
      android.os.Process.killProcess(android.os.Process.myPid());
```

      至于说使用  

```
ActivityManager activityManager =(ActivityManager)context.getSystemService(Context.ACTIVITY_SERVICE);
        activityManager.restartPackage(processName);
```

    这行代码却杀不死进程，为什么我也没弄明白呢，现在就结束了，问题就解决了！！！




备注：部分资料摘自网络，关于Picasso的使用介绍，暂时没有，后续会增加的！