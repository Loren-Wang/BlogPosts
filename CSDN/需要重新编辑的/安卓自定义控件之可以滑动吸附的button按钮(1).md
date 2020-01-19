
功能：滑动、媳妇、设置初始位置、共享存储当前位置、设置滑动范围

```


import android.content.Context;
import android.util.AttributeSet;
import android.util.DisplayMetrics;
import android.view.MotionEvent;
import android.widget.Button;

/**
 * Created by wangliang on 0027/2016/9/27.
 */
public class MoveButton extends Button {
    private int screenWidth;
    private int screenHeight;

    private Integer defaultLeft;
    private Integer defaultTop;
    private Integer defaultRight;
    private Integer defaultBottom;

    public MoveButton(Context context) {
        super(context);
        DisplayMetrics dm=getResources().getDisplayMetrics();
        screenWidth=dm.widthPixels;
        screenHeight=dm.heightPixels;
    }

    public MoveButton(Context context, AttributeSet attrs) {
        super(context, attrs);
        DisplayMetrics dm=getResources().getDisplayMetrics();
        screenWidth=dm.widthPixels;
        screenHeight=dm.heightPixels;
    }

    public MoveButton(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        DisplayMetrics dm=getResources().getDisplayMetrics();
         screenWidth=dm.widthPixels;
         screenHeight=dm.heightPixels;
    }

    public void setLocation(int l, int t, int r, int b){
        this.defaultLeft = l;
        this.defaultTop = t;
        this.defaultRight = r;
        this.defaultBottom = b;
    }

    public void setScreenCcope(int screenWidth,int screenHeight){
        this.screenWidth = screenWidth;
        this.screenHeight = screenHeight;
    }

    int lastX,lastY;
    int downX,downY;
    private boolean whetherScroll = false;
    
    @Override
    public boolean onTouchEvent(MotionEvent event) {
        switch (event.getAction()){
            case MotionEvent.ACTION_DOWN:
            lastX=(int)event.getRawX();//获取触摸事件触摸位置的原始X坐标
            lastY=(int)event.getRawY();
            downX=(int)event.getRawX();//获取触摸事件触摸位置的原始X坐标
            downY=(int)event.getRawY();
            break;

            case MotionEvent.ACTION_MOVE:
                int dx=(int)event.getRawX()-lastX;
                int dy=(int)event.getRawY()-lastY;

                if(Math.abs(event.getRawX() - downX) < 10 && Math.abs(event.getRawY() - downY) < 10){
                    setEnabled(true);
                    return super.onTouchEvent(event);
                }

                setEnabled(false);

                int l=getLeft()+dx;
                int b=getBottom()+dy;
                int r=getRight()+dx;
                int t=getTop()+dy;


                //下面判断移动是否超出屏幕
                if(l<0){
                    l=0;
                    r=l+getWidth();
                }

                if(t<0){
                    t=0;
                    b=t+getHeight();
                }

                if(r>screenWidth){
                    r=screenWidth;
                    l=r-getWidth();
                }

                if(b > screenHeight){
                    b=screenHeight;
                    t=b - getHeight();
                }
                whetherFirstCreateView = false;
                layout(l, t, r, b);

                lastX=(int)event.getRawX();
                lastY=(int)event.getRawY();
                postInvalidate();
                whetherScroll = true;
                break;
            case MotionEvent.ACTION_UP:
                if(Math.abs(event.getRawX() - downX) < 10 && Math.abs(event.getRawY() - downY) < 10){
                    setEnabled(true);
                    return super.onTouchEvent(event);
                }

                dx=(int)event.getRawX()-lastX;
                dy=(int)event.getRawY()-lastY;
                l=getLeft()+dx;
                b=getBottom()+dy;
                r=getRight()+dx;
                t=getTop()+dy;
                if(l <= screenWidth - getWidth() - l) {
                    layout(0, t, getWidth(), b);
                    SharedPrefUtils.putInt(AppCommon.MOVE_BUTTON_LOCATION_LEFT,0);
                    SharedPrefUtils.putInt(AppCommon.MOVE_BUTTON_LOCATION_RIGHT,getWidth());
                }else {
                    layout(screenWidth - getWidth(), t, screenWidth, b);
                    SharedPrefUtils.putInt(AppCommon.MOVE_BUTTON_LOCATION_LEFT,screenWidth - getWidth());
                    SharedPrefUtils.putInt(AppCommon.MOVE_BUTTON_LOCATION_RIGHT,screenWidth);
                }
                setEnabled(true);
                SharedPrefUtils.putInt(AppCommon.MOVE_BUTTON_LOCATION_TOP,t);
                SharedPrefUtils.putInt(AppCommon.MOVE_BUTTON_LOCATION_BOTTOM,b);
                whetherScroll = false;
                return true;
        }
        return super.onTouchEvent(event);
    }

    @Override
    protected void onLayout(boolean changed, int left, int top, int right, int bottom) {
        super.onLayout(changed, left, top, right, bottom);
    }

    boolean whetherFirstCreateView = true;
    @Override
    public void layout(int l, int t, int r, int b) {
        if(whetherFirstCreateView && defaultLeft != null && defaultTop != null && defaultRight != null && defaultBottom != null){
            super.layout(defaultLeft, defaultTop, defaultRight, defaultBottom);
        }else {
            if(whetherScroll && t != 0) {//只有在滑动的情况下才允许重新布局
                super.layout(l, t, r, b);
            }
        }
    }
}

```