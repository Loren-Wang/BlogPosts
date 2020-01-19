```


import android.content.Context;
import android.support.v4.view.ViewPager;
import android.util.AttributeSet;
import android.view.MotionEvent;

public class NotSlipViewPager extends ViewPager {

    private boolean isCanScroll = false;

    public NotSlipViewPager(Context context) {
        super(context);
        setOnPageChangeListener(onPageChangeListener);
    }

    public NotSlipViewPager(Context context, AttributeSet attrs) {
        super(context, attrs);
        setOnPageChangeListener(onPageChangeListener);
    }

    public void setScanScroll(boolean isCanScroll){
        this.isCanScroll = isCanScroll;
    }

    @Override
    public void setCurrentItem(int item) {
        if(getCurrentItem() != item) {
            setScanScroll(true);
            super.setCurrentItem(item);
        }
    }

    @Override
    public void setCurrentItem(int item, boolean smoothScroll) {
        if(getCurrentItem() != item) {
            setScanScroll(true);
            super.setCurrentItem(item,smoothScroll);
        }
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent event) {
        if(isCanScroll){
            return super.onInterceptTouchEvent(event);
        }
        // Never allow swiping to switch between pages
        return false;
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        if(isCanScroll){
            return super.onTouchEvent(event);
        }
        // Never allow swiping to switch between pages
        return false;
    }

    private OnPageChangeListener onPageChangeListener =  new OnPageChangeListener() {
        @Override
        public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {
            if(positionOffset == 0){
                setScanScroll(false);
            }
        }

        @Override
        public void onPageSelected(int position) {

        }

        @Override
        public void onPageScrollStateChanged(int state) {

        }
    };
}
```