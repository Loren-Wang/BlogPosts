   在开发的时候会碰到遮掩的一个问题，将状态栏以及导航栏透明之后，在点击编辑框弹出软件盘的时候会出现软键盘将编辑框挡住了的现象，这个时候修改manifest也不起作用，这是由于透明导航栏状态栏后与软键盘产生了冲突，导致编辑框被遮挡。
解决的方法参考的是[http://blog.csdn.net/silentweek/article/details/53118492](http://blog.csdn.net/silentweek/article/details/53118492)，这篇文章，但是我做了一些修改，我将向上移的布局修改为根布局，就是说activity最外层布局，**获取最外层布局的集中方法如下：**
```
getParent(获取上一级View)
getRootView
getWindow().getDecorView()
findViewById(android.R.id.content)
((ViewGroup)findViewById(android.R.id.content)).getChildAt(0)
```

最后总结下来的代码如下：

```

    @Override
    protected void onCreate(Bundle savedInstanceState) {
    。。。。
            setContentView(R.layout.activity_base);
        //沉浸式通知栏使用（以下三个语句必须同时启用或同时关闭）
        //透明状态栏
        getWindow().addFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS);
        //透明导航栏
        getWindow().addFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_NAVIGATION);
        //设置透明状态栏以及导航栏与软键盘弹出的冲突问题（软键盘弹出收回监听）
        getWindow().getDecorView().getViewTreeObserver().addOnGlobalLayoutListener(onGlobalLayoutListener);

。。。。

}
    private ViewTreeObserver.OnGlobalLayoutListener onGlobalLayoutListener = new ViewTreeObserver.OnGlobalLayoutListener() {
        private View rootView = null;
        @Override
        public void onGlobalLayout() {
            if(!CheckUtils.isNotEmptyOrNull(rootView)){//检测是否为空
                rootView = findViewById(android.R.id.content);
            }
            if(CheckUtils.isNotEmptyOrNull(rootView)) {
                Rect rect = new Rect();
                getWindow().getDecorView().getWindowVisibleDisplayFrame(rect);
                int screenHeight = getScreenHeight();
                int heightDifference = screenHeight - rect.bottom;//计算软键盘占有的高度  = 屏幕高度 - 视图可见高度

                if (rootView.getLayoutParams() instanceof RelativeLayout.LayoutParams) {
                    RelativeLayout.LayoutParams layoutParams = (RelativeLayout.LayoutParams) rootView.getLayoutParams();
                    layoutParams.setMargins(0, 0, 0, heightDifference);//设置rlContent的marginBottom的值为软键盘占有的高度即可
                } else if (rootView.getLayoutParams() instanceof LinearLayout.LayoutParams) {
                    LinearLayout.LayoutParams layoutParams = (LinearLayout.LayoutParams) rootView.getLayoutParams();
                    layoutParams.setMargins(0, 0, 0, heightDifference);//设置rlContent的marginBottom的值为软键盘占有的高度即可
                }
                rootView.requestLayout();
            }
        }


        private int getScreenHeight(){
            WindowManager manager = getWindowManager();
            DisplayMetrics outMetrics = new DisplayMetrics();
            manager.getDefaultDisplay().getMetrics(outMetrics);
            int height = outMetrics.heightPixels;
            return  height;
        }
    };
```

最后不要忘了在界面销毁的时候移除监听：

```
    @Override
    protected void onDestroy() {
        super.onDestroy();
        //退出looper
        if(android.os.Build.VERSION.SDK.compareTo("18") < 0) {
            handlerThread.quit();
        }else {
            handlerThread.quitSafely();
        }
        //移除软键盘弹出收回监听（解决隐藏状态栏以及导航栏导致和软件盘冲突的解决所添加的监听）
        if(CheckUtils.isNotEmptyOrNull(onGlobalLayoutListener)){
            getWindow().getDecorView().getViewTreeObserver().removeOnGlobalLayoutListener(onGlobalLayoutListener);
        }
    }
```