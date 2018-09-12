
上方显示

```
private void showPopUp(View v) {
        LinearLayout layout = new LinearLayout(this);
        layout.setBackgroundColor(Color.GRAY);
        TextView tv = new TextView(this);
        tv.setLayoutParams(new LayoutParams(LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT));
        tv.setText("I'm a pop -----------------------------!");
        tv.setTextColor(Color.WHITE);
        layout.addView(tv);
 
        popupWindow = new PopupWindow(layout,120,120);
         
        popupWindow.setFocusable(true);
        popupWindow.setOutsideTouchable(true);
        popupWindow.setBackgroundDrawable(new BitmapDrawable());
         
        int[] location = new int[2];
        v.getLocationOnScreen(location);
         
        popupWindow.showAtLocation(v, Gravity.NO_GRAVITY, location[0], location[1]-popupWindow.getHeight());
    }
```

下方

```
popupWindow.showAsDropDown(v)
```

左方

```
popupWindow.showAtLocation(v, Gravity.NO_GRAVITY, location[0]-popupWindow.getWidth(), location[1]);
```

右方

```
popupWindow.showAtLocation(v, Gravity.NO_GRAVITY, location[0]+v.getWidth(), location[1]);
```


转自：http://my.oschina.net/zhulunjun/blog/260859