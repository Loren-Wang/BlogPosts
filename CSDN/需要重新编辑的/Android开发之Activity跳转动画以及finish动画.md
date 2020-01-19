Android默认的activity跳转是不带动画的，但是如果想要动画的话是有两种方法的，一种是代码中加入，另外一种是写在布局中的theme中的

第一种：
----

代码中加入，在startActivity或者finish之后加入

```
ps:inAnimResId,outAnimResId 这两个是动画的资源id，也就是在res/anim/下的文件，详细点就是R.anim.inAnimResId,R.anim.inAnimResId

Activity().overridePendingTransition(inAnimResId,outAnimResId);  
```

第二种：
----
在style.xml 中加入同时在AndroidManifest.xml中将Application的主题修改为ThemeActivity，如果不想改变所有Activity，可以单独设置每个Activity的theme

```
    <!--当我们从 A1 启动 A2 时，A1 从屏幕上消失，这个动画叫做 android:activityOpenExitAnimation-->
    <!--当我们从 A1 启动 A2 时，A2 出现在屏幕上，这个动画叫做 android:activityOpenEnterAnimation-->
    <!--当我们从 A2 退出回到 A1 时，A2 从屏幕上消失，这个叫做 android:activityCloseExitAnimation-->
    <!--当我们从 A2 退出回到 A1 时，A1 出现在屏幕上，这个叫做 android:activityCloseEnterAnimation-->
    <style name="nimation" parent="@android:style/Animation.Activity">
        <item name="android:activityOpenEnterAnimation">@anim/frame_anim_from_popu_in</item>
        <item name="android:activityOpenExitAnimation">@anim/frame_anim_from_popu_out</item>
        <item name="android:activityCloseEnterAnimation">@anim/frame_anim_from_popu_in</item>
        <item name="android:activityCloseExitAnimation">@anim/frame_anim_from_popu_out</item>
    </style>


修改AndroidManifest.xml
android:theme="@style/ThemeActivity"  
```

