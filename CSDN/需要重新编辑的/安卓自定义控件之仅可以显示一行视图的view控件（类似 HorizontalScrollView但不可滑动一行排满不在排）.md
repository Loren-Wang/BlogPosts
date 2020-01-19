该控件的功能是显示一行视图，这一行可以无限添加view，以及添加Headview、footview；当本行呗view排列满之后不在排列，如果添加了footview，那么footview肯定会在最后显示。
代码如下：

```

import android.app.Activity;
import android.content.Context;
import android.util.AttributeSet;
import android.util.DisplayMetrics;
import android.view.View;
import android.view.ViewGroup;

import java.util.ArrayList;
import java.util.List;

/**
 * Created by wangliang on 0022/2016/9/22.
 * 功能：显示一行view，水平的，多个view，最多显示一行
 * 思路：动态添加view，如果添加了尾view的话，则在绘制onlayout的时候余留除尾view的空间，
 * 所有的view取最小view的高度进行压缩显示显示足一整行，显示不足就显示能显示的部分，显示超过，则就显示一行
 */
public class MyHorizontalView extends ViewGroup {
    private Context context;
    private int windowWidth = 0;
    private int nowAllViewWidth = 0;
    private boolean whetherAddHeadView = false;
    private boolean whetherAddFootView = false;
    private String TAG = "MyHorizontalView";
    private List<View> allViews = new ArrayList<>();
    private int maxHeight = 0;
    private View footView;

    public MyHorizontalView(Context context) {
        super(context);
        initSetting(context);
    }

    public MyHorizontalView(Context context, AttributeSet attrs) {
        this(context, attrs,0);
    }

    public MyHorizontalView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        initSetting(context);
    }

    private void initSetting(Context context){
        this.context = context;
        DisplayMetrics dm = new DisplayMetrics();
        //取得窗口属性
        ((Activity)context).getWindowManager().getDefaultDisplay().getMetrics(dm);
        windowWidth = dm.widthPixels;

    }

    /**
     * 添加头view
     * @param view
     */
    public void addHeadView(View view, ViewGroup.LayoutParams params){
        if(!whetherAddHeadView) {
            whetherAddHeadView = true;
            allViews.add(0,view);
            super.addView(view, -1,params);
        }
    }

    /**
     * 添加末尾view
     * @param view
     */
    public void addFootView(View view, ViewGroup.LayoutParams params){
        if(!whetherAddFootView) {
            whetherAddFootView = true;
            footView = view;
            super.addView(view, -1,params);
        }
    }

    @Override
    public void addView(View child, int index, ViewGroup.LayoutParams params) {
        super.addView(child, -1,params);
        allViews.add(child);
    }

    @Override
    public void addView(View child) {
        addView(child,-1,null);
    }

    @Override
    public void addView(View child, int index) {
        addView(child, index,null);
    }

    @Override
    public void addView(View child, LayoutParams params) {
        addView(child,-1, params);
    }

    @Override
    protected LayoutParams generateLayoutParams(
            LayoutParams p)
    {
        return new MarginLayoutParams(p);
    }

    @Override
    public LayoutParams generateLayoutParams(AttributeSet attrs)
    {
        return new MarginLayoutParams(getContext(), attrs);
    }

    @Override
    protected LayoutParams generateDefaultLayoutParams()
    {
        return new MarginLayoutParams(LayoutParams.MATCH_PARENT,
                LayoutParams.MATCH_PARENT);
    }

    /**
     * 负责设置子控件的测量模式和大小 根据所有子控件设置自己的宽和高
     */
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec)
    {
        int minHeight = Integer.MAX_VALUE ;

        // 遍历每个子元素
        for (int i = 0; i < getChildCount(); i++)
        {
            View child = getChildAt(i);
            // 测量每一个child的宽和高
            child.measure(0,0);
            // 得到child的lp
            MarginLayoutParams lp = (MarginLayoutParams) child
                    .getLayoutParams();

            minHeight = Math.min(lp.height  + lp.topMargin
                    + lp.bottomMargin,minHeight);

        }
        setMeasuredDimension(widthMeasureSpec, Math.max(minHeight,heightMeasureSpec));

    }

    private boolean whetherLayout;
    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {

        if(whetherLayout){
            whetherLayout = false;
        }else {
            whetherLayout = true;
            return;
        }

        //判断是否有尾视图，如果有则需要计算尾视图，为尾视图预留显示空间
        int footViewWidth = 0;
        int footViewBc = 0;
        int footViewLc = 0;
        int footViewTc = 0;
        int footViewRc = 0;
        if(whetherAddFootView && footView != null){
            MarginLayoutParams lp = (MarginLayoutParams) footView.getLayoutParams();
            footView.measure(0,0);
            double beiShu = getHeight() * 1.0 / (footView.getMeasuredHeight() + lp.topMargin + lp.bottomMargin);
            footViewLc = (int) (lp.leftMargin * beiShu);
            footViewTc = (int) (lp.topMargin * beiShu);
            footViewRc = (int) (footViewLc + (footView.getMeasuredWidth() * beiShu));
            footViewBc = (int) (footViewTc + (footView.getMeasuredHeight() * beiShu));
            footViewWidth = footViewRc - footViewLc;
        }
        //判断是否有尾视图，如果有则需要计算尾视图，为尾视图预留显示空间

        //绘制尾视图其他部分
        int left = 0;
        int nowWidth = 0;
        for(int i = 0 ; i < allViews.size() ; i++){
            View child = allViews.get(i);
            child.measure(0,0);
            if (child.getVisibility() == View.GONE)
            {
                continue;
            }
            MarginLayoutParams lp = (MarginLayoutParams) child
                    .getLayoutParams();
            double beiShu = getMeasuredHeight() * 1.0 / (child.getMeasuredHeight() + lp.topMargin + lp.bottomMargin);
            //计算childView的left,top,right,bottom
            int lc = (int) (left + lp.leftMargin * beiShu);
            int tc = (int) (lp.topMargin * beiShu);
            int rc = (int) (lc + child.getMeasuredWidth() * beiShu);
            int bc = (int) (tc + child.getMeasuredHeight() * beiShu);
            nowWidth += child.getMeasuredWidth() * beiShu + lp.rightMargin * beiShu
                    + lp.leftMargin * beiShu;
            if(nowWidth + footViewWidth > getMeasuredWidth()){
                break;
            }

            child.layout(lc, tc, rc, bc);
            left += child.getMeasuredWidth() * beiShu + lp.rightMargin * beiShu
                    + lp.leftMargin * beiShu;
        }
        //绘制尾视图其他部分

        //绘制尾视图
        if(whetherAddFootView && footView != null){
            footView.layout(left + footViewLc, footViewTc, left + footViewRc, footViewBc);
        }

    }
}

```



调用代码（其中RoundRectangleImageView可以更换为其他view）：

```
ViewGroup.MarginLayoutParams lp = null;
        lp = new ViewGroup.MarginLayoutParams(
                80, 80);
        lp.leftMargin = 10;
        lp.rightMargin = 10;
        lp.topMargin = 50;
        lp.bottomMargin = 50;


        RoundRectangleImageView roundRectangleImageView = new RoundRectangleImageView(getContext());
        Picasso.with(getContext()).load(R.drawable.people_head)
                .placeholder(R.drawable.ic_launcher).error(R.drawable.ic_launcher).resize(500,500).into(roundRectangleImageView);
        hv.addHeadView(roundRectangleImageView,lp);
        hv.addFootView(roundRectangleImageView,lp);


        for(int i = 0; i < 10 ; i++){
            roundRectangleImageView = new RoundRectangleImageView(getContext());

            Picasso.with(getContext()).load(R.drawable.ic_launcher)
                    .placeholder(R.drawable.ic_launcher).error(R.drawable.ic_launcher).resize(500,500).into(roundRectangleImageView);
            hv.addView(roundRectangleImageView,lp);
        }
```