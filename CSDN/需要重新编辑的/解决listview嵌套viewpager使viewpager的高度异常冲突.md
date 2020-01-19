思路就是在 onMeasure 方法中计算子控件的最大高度：
使用getChildCount获得所有子控件，在遍历子控件，通过子控件的getMeasuredHeight方法获得子控件高度，之前布局子控件，在进行比较设置给viewpager；点击事件冲突使用：getParent().requestDisallowInterceptTouchEvent(true);//通知父view，你处理你的事件，我处理我的事件。方法；

```
 @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        int height = 0;
        for (int i = 0; i < getChildCount(); i++) {
            View view = getChildAt(i);
            view.measure(widthMeasureSpec, MeasureSpec.makeMeasureSpec(0, MeasureSpec.UNSPECIFIED));
            height = Math.max(view.getMeasuredHeight(),height);
        }
        heightMeasureSpec = MeasureSpec.makeMeasureSpec(height, MeasureSpec.AT_MOST);
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
    }

        @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
        getParent().requestDisallowInterceptTouchEvent(true);//这句话的作用 告诉父view，我的单击事件我自行处理，不要阻碍我。
        return super.dispatchTouchEvent(ev);
    }
```