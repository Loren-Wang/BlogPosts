> 参考自张鸿洋博客：http://blog.csdn.net/lmj623565791/article/details/45059587

对于添加分割线的方法进行了一些修改，是的分割线的样式可以进行动态的传入，当然鸿阳大神的那个分割线样式也保留下来了，只不过是作为了默认样式的存在；直接上代码：

```

import android.content.Context;
import android.content.res.TypedArray;
import android.graphics.Canvas;
import android.graphics.Rect;
import android.graphics.drawable.Drawable;
import android.support.v7.widget.LinearLayoutManager;
import android.support.v7.widget.RecyclerView;
import android.view.View;

/**
 * 创建人：王亮
 * 创建时间：2016/12/21
 * 功能作用：用于recycleview的分割线的绘制，可以使用默认视图样式，也可以使用传入的样式
 */

public class RecyclerViewItemDecoration extends RecyclerView.ItemDecoration{
    private static final int[] ATTRS = new int[]{
            android.R.attr.listDivider
    };

    public static final int HORIZONTAL_LIST = LinearLayoutManager.HORIZONTAL;

    public static final int VERTICAL_LIST = LinearLayoutManager.VERTICAL;

    private View ItemDecorationView;//间隔视图
    private Drawable ItemDecorationViewDrawble;//间隔视图的drawble

    private int ItemDecorationViewWidth = 0;//间隔视图的宽度
    private int ItemDecorationViewHeight = 0;//间隔视图的高度

    private int mOrientation;

    public RecyclerViewItemDecoration(Context context, int orientation,View ItemDecorationView) {

        //判断传入的view以及传入的view的params是否为空，若其中一个为空则使用默认样式
        if(ItemDecorationView == null || ItemDecorationView.getLayoutParams() == null){
            final TypedArray a = context.obtainStyledAttributes(ATTRS);
            this.ItemDecorationViewDrawble = a.getDrawable(0);
            this.ItemDecorationView = null;
            a.recycle();
            ItemDecorationViewWidth = this.ItemDecorationViewDrawble.getIntrinsicWidth();
            ItemDecorationViewHeight = this.ItemDecorationViewDrawble.getIntrinsicHeight();

        }else {
            this.ItemDecorationView = ItemDecorationView;
            this.ItemDecorationView.measure(0,0);
            this.ItemDecorationViewDrawble = ItemDecorationView.getBackground();

            if(this.ItemDecorationView.getMeasuredHeight() >= 0){
                ItemDecorationViewHeight = this.ItemDecorationView.getMeasuredHeight();
            }
            if(this.ItemDecorationView.getMeasuredWidth() >= 0){
                ItemDecorationViewWidth = this.ItemDecorationView.getMeasuredWidth();
            }
            if(ItemDecorationView.getLayoutParams().height >= 0){
                ItemDecorationViewHeight = ItemDecorationView.getLayoutParams().height;
            }
            if(ItemDecorationView.getLayoutParams().width >= 0){
                ItemDecorationViewWidth = ItemDecorationView.getLayoutParams().width;
            }
        }
        setOrientation(orientation);
    }

    public void setOrientation(int orientation) {
        if (orientation != HORIZONTAL_LIST && orientation != VERTICAL_LIST) {
            throw new IllegalArgumentException("invalid orientation");
        }
        mOrientation = orientation;
    }

    @Override
    public void onDraw(Canvas c, RecyclerView parent) {

        if (mOrientation == VERTICAL_LIST) {
            drawVertical(c, parent);
        } else {
            drawHorizontal(c, parent);
        }

    }


    public void drawVertical(Canvas c, RecyclerView parent) {
        final int left = parent.getPaddingLeft();
        final int right = parent.getWidth() - parent.getPaddingRight();

        final int childCount = parent.getChildCount();
        for (int i = 0; i < childCount; i++) {
            final View child = parent.getChildAt(i);
            android.support.v7.widget.RecyclerView v = new android.support.v7.widget.RecyclerView(parent.getContext());
            final RecyclerView.LayoutParams params = (RecyclerView.LayoutParams) child
                    .getLayoutParams();
            final int top = child.getBottom() + params.bottomMargin;
            final int bottom  = top +  ItemDecorationViewHeight;
            ItemDecorationViewDrawble.setBounds(left, top, right, bottom);
            ItemDecorationViewDrawble.draw(c);//在drable上绘制itemview
        }
    }

    public void drawHorizontal(Canvas c, RecyclerView parent) {
        final int top = parent.getPaddingTop();
        final int bottom = parent.getHeight() - parent.getPaddingBottom();

        final int childCount = parent.getChildCount();
        for (int i = 0; i < childCount; i++) {
            final View child = parent.getChildAt(i);
            final RecyclerView.LayoutParams params = (RecyclerView.LayoutParams) child
                    .getLayoutParams();
            final int left = child.getRight() + params.rightMargin;
            final int right = left + ItemDecorationViewHeight;
            ItemDecorationViewDrawble.setBounds(left, top, right, bottom);
            ItemDecorationViewDrawble.draw(c);//在drable上绘制itemview
        }
    }

    @Override
    public void getItemOffsets(Rect outRect, int itemPosition, RecyclerView parent) {
        //设置itemview的偏移量
        if (mOrientation == VERTICAL_LIST) {
            outRect.set(0, 0, 0, ItemDecorationViewHeight);
        } else {
            outRect.set(0, 0, ItemDecorationViewWidth, 0);
        }
    }
}
```