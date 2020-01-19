1.软键盘弹出导致背景变形：
--------------

  

```
LinearLayout 有没在    ScrollView 下面？有的话，添加属性给scrollview


<ScrollView
android:scrollbars="horizontal">

<LinearLayout 
。。。
>
</LinearLayout>

</ScrollView>
```

2.进制转换
------

```
1	// 十进制转化为十六进制，结果为C8。
2	Integer.toHexString(200);
3	 
4	// 十六进制转化为十进制，结果140。
5	Integer.parseInt("8C",16);

```

3.app在as上第一次安装编译启动时间慢
---------------------

```
先检查as的instant run的状态
```


4.在软键盘弹出的时候需要底部控件上移显示的方法
------------------------

```
   解决：将要固定不上移的部分放入到scrollview中，将要跟随上移的部分放到scrollview外部就行
```

5.
--

```
baseadapter 中的getViewTypeCount返回view类型数量的方法的最大值要小于getItemViewType的返回值，例如：getItemViewType的返回值是6，那么getItemViewType返回的最大值要小于6；
```

6.百度地图添加自定义view视图的方法
--------------------

```
        MapViewLayoutParams.Builder builder = new  MapViewLayoutParams.Builder();
        builder.width(200);
        builder.height(200);
        builder.position(new LatLng(31.242102,121.374508));
        builder.layoutMode(MapViewLayoutParams.ELayoutMode.mapMode);
        ImageView imageView = new ImageView(getContext());
        imageView.setImageResource(R.drawable.ic_launcher_app_logo);
        imageView.setLayoutParams(new ViewGroup.LayoutParams(200,200));
        mapView.addView(imageView,builder.build());
```


7.字符串大小写转换
----------

```
    转为大写：str.toUpperCase()
    转为小写：str.toLowerCase()
```


8.listview中addHeadView以及addFootView使用注意
---------------------------------------

```
在API18之前使用需要在setAdapter之前调用
```


9.获取recycleview在垂直方向滑动的距离
-------------------------

（ps:前提是LayoutManager用的是LinearLayoutManager）

```
public int getScollYDistance() {  
    LinearLayoutManager layoutManager = (LinearLayoutManager) this.getLayoutManager();  
    int position = layoutManager.findFirstVisibleItemPosition();  
    View firstVisiableChildView = layoutManager.findViewByPosition(position);  
    int itemHeight = firstVisiableChildView.getHeight();  
    return (position) * itemHeight - firstVisiableChildView.getTop();  
}  
```

10.总结和分析几种判断RecyclerView到达底部的方法
-------------------------------
[从网上看到的一篇文章，对于recycleview的一些高度的处理感觉比较有用：http://www.open-open.com/lib/view/open1474266969512.html](http://www.open-open.com/lib/view/open1474266969512.html)

11.Android debug.keystore的密码
----------------------------
       不管是Android Studio 还是Eclipse进行移动App的开发，在井陉编译的时候都会有一个生成一个默认的签名文件，他的默认名称是“debug.keystore”，他的默认路径是“C:\Users\<用户名>\.Android\debug.keystore”，他的密码是“android”

12.Android Stduio统计项目的代码行数
--------------------------
参考链接：[http://www.cnblogs.com/common1140/p/5016244.html](http://www.cnblogs.com/common1140/p/5016244.html)

      android studio统计项目的代码行数的步骤如下：
      1）按住Ctrl+Shift+A，在弹出的框输入‘find’,然后选择Find in Path.(或者使用快捷键Ctrl+Shift+F)
      2）在弹出Find in Path的框中的Text to find输入\n,接着勾选Regular expression(正则表达式)，Context选择anywhere，Scope根据你想要统计的范围进行选择，File mask选择*.java。（在这里统计项目的Java的代码行数）
      3）下图的Useages in generated code是自动生成的代码，上面那个就是项目的代码行数。


##13.华为手机等拥有虚拟导航栏的手机对于app底部遮挡的问题
  在oncreate中检测是否有以下语句 ：
  

```
//透明导航栏
getWindow().addFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_NAVIGATION);
```

##14.界面更新问题
  如果对于数据进行操作的结果影响到了其他的界面，那么是可以通过其他界面的上下文对其数据逻辑等操作进行更新！




##15.merge的使用中的一个坑
   在使用代码inflate引入merge布局的时候不要相信返回的view，这个返回的view只是第一次使用inflate引入merge的view，后续引入相同的merge返回的还是第一次的


##16.自定义alerdialog横向充满屏幕方法（必须先显示然后在设置横向，否则无效）

      //显示弹窗
       show();
      //设置横向全屏
      getWindow().setAttributes(lp);