定义：嵌套在scrollview或listview中时viewpager的高度需要固定，但是每一个item的高度都不相同，所以在viewpager切换结束的时候重置viewpager高度
```
import android.content.Context;
import android.support.v4.view.ViewPager;
import android.util.AttributeSet;
import android.view.View;

/**
 * Created by wangliang on 0018/2016/8/18.
 */
public class MyViewPager extends ViewPager {


    public MyViewPager(Context context) {
        super(context);
    }

    public MyViewPager(Context context, AttributeSet attrs) {
        super(context, attrs);
    }


    private int widthMeasureSpec;
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        this.widthMeasureSpec = widthMeasureSpec;
        if(getChildCount() > 0) {
            View view = getChildAt(0);
            view.measure(widthMeasureSpec, MeasureSpec.makeMeasureSpec(0, MeasureSpec.UNSPECIFIED));
            super.onMeasure(widthMeasureSpec, MeasureSpec.makeMeasureSpec(view.getMeasuredHeight(), MeasureSpec.AT_MOST));
        }else {
            super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        }

    }

    /**
     * 在切换tab的时候，重置ViewPager的高度
     */
        public void resetViewHeightToShowViewHeight(View showView){
            showView.measure(widthMeasureSpec, MeasureSpec.makeMeasureSpec(0, MeasureSpec.UNSPECIFIED));
            super.onMeasure(widthMeasureSpec, MeasureSpec.makeMeasureSpec(showView.getMeasuredHeight(), MeasureSpec.AT_MOST));
            layout(getLeft(),getTop(),getRight(), (int) (showView.getMeasuredHeight() + getY()));
    }




}

```


调用：

```
 @Override
        public void onPageScrollStateChanged(int state) {
            if( state == 0 && lvShowDetail.getFirstVisiblePosition() != 0) {
                lvShowDetail.smoothScrollToPosition(0);
            }

            if(vpgShowDetail != null && h1 != null && h2 != null) {
                switch (vpgShowDetail.getCurrentItem()) {
                    case 0:
                        vpgShowDetail.resetViewHeightToShowViewHeight(View);
                        break;
                    case 1:
                        vpgShowDetail.resetViewHeightToShowViewHeight(lv);
                        break;
                }
            }
        }
```