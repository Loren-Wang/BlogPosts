```

import android.content.Context;
import android.graphics.Paint;
import android.support.v4.view.ViewPager;
import android.text.TextPaint;
import android.util.AttributeSet;
import android.view.Gravity;
import android.view.View;
import android.view.ViewGroup;
import android.widget.LinearLayout;
import android.widget.RelativeLayout;
import android.widget.TextView;


import java.util.ArrayList;
import java.util.List;


/**
 * 创建时间：2017年1月3日
 * 创建人：王亮
 * 思路：
 *    （1）布局方式：最外层父布局为LinearLayout布局，第一层子布局是两个水平排列的线性布局：
 *        第一个水平排列布局室友多个相同权重比的textview组成，第二个水平排列布局室友一个textview组成
 *    （2）初始化子布局：当传入viewpager以及tablist的时候不进行布局的初始化，只有在onmesuse方法中获得父容器的宽度的时候在进行初始化
 *    （3）原理：切换的效果是由第二层水平排列的布局产生，在初始化子视图的时候会计算好所有的文字的宽度并存到集合当中，
 *         当viewpager进行滑动的时候根据positionOffset以及position进行第二个水平排列布局的左侧内边距的计算，
 *         由于第一层标题是等比的，所以机选有当前的(position)乘以等比出来的宽度加上等比宽度乘以移动的百分比positionOffset的结果
 *         再加上初始的paddingleft就是所向左侧或右侧移动的距离
 *
 * 注意：传入的viewpage的页面滑动监听只能在本视图里注册，不可再调用本视图的页面注册，
 *      如果要在调用本视图的页面注册页面滑动监听则使用本页面的接口产生的页面滑动监听
 */
public class MyTabLayout extends LinearLayout {
    private String TAG;
    private int tabTextViewColor;//tab文本显示颜色
    private int tabTextViewSize;//tab文本显示大小
    private int tabSelectColor;//选中的文本以及下划线颜色
    private int lineHeight;//切换线的高度

    private Context context;
    private List<TextView> textViewList = new ArrayList<>();
    private LinearLayout lineView;
    private List<String>  tabTitleList;
    private ViewPager viewPager;

    private MyTabLayoutOnPageChangeListener myTabLayoutOnPageChangeListener;//页面的切换监听
    private int viewWidth = 0;
    private int viewHeight = 0;


    public MyTabLayout(Context context) {
        super(context);
        init(context);
    }

    public MyTabLayout(Context context, AttributeSet attrs) {
        super(context, attrs);
        init(context);
    }

    public MyTabLayout(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(context);
    }

    /**
     * 设置默认的初始属性
     * @param context
     */
    private void init(Context context){
        this.context = context;
        TAG = getClass().getName();
        setOrientation(VERTICAL);//设置布局垂直显示
        setGravity(Gravity.CENTER_VERTICAL);
        tabTextViewColor = context.getResources().getColor(R.color.actionbar_bg);
        tabTextViewSize = (int) CommonUtils.getInstance().getSpResourceValue(getContext(),R.dimen.app_text_size_5);
        tabSelectColor = context.getResources().getColor(R.color.app_text_color_4);
        lineHeight = 5;
        lineView = new LinearLayout(getContext());
        lineView.setLayoutParams(new LinearLayout.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT));
        lineView.setOrientation(HORIZONTAL);

        //获取文本宽度的画笔
        TextPaint paint = new TextPaint();
        float scaledDensity = context.getResources().getDisplayMetrics().scaledDensity;
        paint.setTextSize(scaledDensity * tabTextViewSize);
        Paint.FontMetrics fontMetrics = paint.getFontMetrics();


    }

    private boolean isLoading = false;//是否加载过

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        if(getWidth() == 0){
            if(getMeasuredWidth() == 0){
                if(viewWidth == 0){
                    viewWidth = 0;
                }
            }else {
                viewWidth = getMeasuredWidth();
            }
        }else {
            viewWidth = getWidth();
        }

        if(getHeight() == 0){
            if(getMeasuredHeight() == 0){
                if(viewHeight == 0){
                    viewHeight = 0;
                }
            }else {
                viewHeight = getMeasuredHeight();
            }
        }else {
            viewHeight = getHeight();
        }

        if(viewWidth != 0 && viewHeight != 0){
            //两个输入是否为空；是否获得了父容器的高度；是否已经加载过子视图了;标题和视图容量是否相同

            if(tabTitleList != null && viewPager != null && viewWidth != 0 && viewHeight != 0 && !isLoading ) {
                isLoading = true;

                LogUtils.logD(TAG + " viewHeight:",viewHeight + "");

                //记录所有的文字的宽度
                final List<Float> textWidthList = new ArrayList<>();

                //初始化标题栏父容器
                LinearLayout linearLayout = new LinearLayout(getContext());
                linearLayout.setLayoutParams(new LinearLayout.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT));
                linearLayout.setOrientation(HORIZONTAL);
                linearLayout.measure(0,0);

                //获取文本宽度的画笔
                TextPaint paint = new TextPaint();
                float scaledDensity = context.getResources().getDisplayMetrics().scaledDensity;
                paint.setTextSize(scaledDensity * tabTextViewSize);
                Paint.FontMetrics fontMetrics = paint.getFontMetrics();


                //初始化tab标题栏
                int maxTextWidth = 0;
                LinearLayout.LayoutParams textParams = new LinearLayout.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, viewHeight - lineHeight);
                textParams.weight = 1;
                textParams.gravity = Gravity.CENTER;
                TextView textView = null;
                for(int  i = 0 ; i < tabTitleList.size() ; i++){
                    textView = new TextView(getContext());
                    textView.setLayoutParams(textParams);
                    textView.setTextColor(tabTextViewColor);
                    textView.setTextSize(tabTextViewSize);
                    textView.setText(tabTitleList.get(i));
                    textView.setGravity(Gravity.CENTER);
                    textWidthList.add(paint.measureText(tabTitleList.get(i)));
                    if(maxTextWidth < paint.measureText(tabTitleList.get(i))){
                        maxTextWidth = (int) paint.measureText(tabTitleList.get(i));
                    }
                    final int finalI = i;
                    textView.setOnClickListener(new OnClickListener() {
                        @Override
                        public void onClick(View v) {
                            if(finalI < viewPager.getChildCount()) {
                                viewPager.setCurrentItem(finalI);
                            }
                        }
                    });
                    textViewList.add(textView);
                    linearLayout.addView(textView);
                }

                //设置tab切换的线
                RelativeLayout.LayoutParams lineParams = new RelativeLayout.LayoutParams(maxTextWidth, lineHeight);
                textView = new TextView(getContext());
                textView.setLayoutParams(lineParams);
                textView.setTextColor(tabTextViewColor);
                textView.setTextSize(tabTextViewSize);
                textView.setGravity(Gravity.CENTER);
                textView.setBackgroundColor(tabSelectColor);
                lineView.addView(textView);

                //设置初始选中
                textViewList.get(0).setTextColor(tabSelectColor);

                //添加两个视图到父容器
                addView(linearLayout);
                addView(lineView);


                final double paddingLeft = (viewWidth * 1.0 / tabTitleList.size() - maxTextWidth) / 2;
                lineView.setPadding((int) paddingLeft,0,0,0);

                //添加页面切换监听
                viewPager.addOnPageChangeListener(new ViewPager.OnPageChangeListener() {
                    @Override
                    public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {
                        LogUtils.logD(TAG + ":position ",position + "");
                        LogUtils.logD(TAG + ":positionOffset ",positionOffset + "");
                        LogUtils.logD(TAG + ":positionOffsetPixels ",positionOffsetPixels + "");

                        int addLeft = 0;//计算左侧间距
                        for(int i = 0 ; i < position + 1 ; i++){
                            if(position == i){//viewpager向左滑动时，position为当前页码-1，享有滑动时为当前页码
                                addLeft += (viewWidth * 1.0 / tabTitleList.size())* positionOffset;
                            }else {
                                addLeft += viewWidth * 1.0 / tabTitleList.size();
                            }
//                        if(position == i){//viewpager向左滑动时，position为当前页码-1，享有滑动时为当前页码
//                            if(position == tabTitleList.size() - 1){
//                                addLeft += (viewWidth * 1.0 / tabTitleList.size())* positionOffset;
//                            }else {
//                                addLeft += (viewWidth * 1.0 / tabTitleList.size()
//                                        - textWidthList.get(position + 1) / 2//本句以及下一句是为了适配不同文字宽度标题
//                                        + textWidthList.get(position) / 2)
//                                        * positionOffset;
//                            }
//                        }else {
//                            if(position == tabTitleList.size() - 1){
//                                addLeft += viewWidth * 1.0 / tabTitleList.size();
//                            }else {
//                                addLeft += viewWidth * 1.0 / tabTitleList.size()
//                                        + textWidthList.get(position + 1) / 2
//                                        - textWidthList.get(position) / 2;
//                            }
//                        }
                        }
                        lineView.setPadding((int) (addLeft + paddingLeft),0,0,0);

                        if(myTabLayoutOnPageChangeListener != null){
                            myTabLayoutOnPageChangeListener.onPageScrolled(position,positionOffset,positionOffsetPixels);
                        }

                    }

                    @Override
                    public void onPageSelected(int position) {
                        for(int i = 0 ; i < textViewList.size() ; i++){
                            if(i == position){
                                textViewList.get(i).setTextColor(tabSelectColor);
                            }else {
                                textViewList.get(i).setTextColor(tabTextViewColor);
                            }
                        }

                        if(myTabLayoutOnPageChangeListener != null){
                            myTabLayoutOnPageChangeListener.onPageSelected(position);
                        }

                    }

                    @Override
                    public void onPageScrollStateChanged(int state) {
                        LogUtils.logD(TAG + ":state ",state + "");
                        if(myTabLayoutOnPageChangeListener != null){
                            myTabLayoutOnPageChangeListener.onPageScrollStateChanged(state);
                        }

                    }
                });
            }
        }else {
            postInvalidate();
        }

        viewHeight = getMeasuredHeight();
    }

    public void setViewPagerTabTitle(final List<String> tabTitleList, ViewPager viewPager, MyTabLayoutOnPageChangeListener myTabLayoutOnPageChangeListener){
        this.tabTitleList = tabTitleList;
        this.viewPager = viewPager;
        this.myTabLayoutOnPageChangeListener = myTabLayoutOnPageChangeListener;
        isLoading = false;
        invalidate();
    }

    /**
     * 设置默认属性
     * @param tabTextViewColor
     * @param tabTextViewSize
     * @param tabSelectColor
     */
    public void setViewRules(Integer tabTextViewColor,Integer tabTextViewSize,Integer tabSelectColor,Integer lineHeight){
        this.tabTextViewColor = tabTextViewColor != null ? tabTextViewColor : this.tabSelectColor;
        this.tabTextViewSize = tabTextViewSize != null ? tabTextViewSize : this.tabTextViewSize;
        this.tabSelectColor = tabSelectColor != null ? tabSelectColor : this.tabSelectColor;
        this.lineHeight = lineHeight != null ? lineHeight : this.lineHeight;
    }

    /**
     * 本页面viewpager滑动监听
     */
    public interface MyTabLayoutOnPageChangeListener{
        void onPageScrolled(int position, float positionOffset, int positionOffsetPixels);
        void onPageSelected(int position);
        void onPageScrollStateChanged(int state);
    }


    @Override
    public void setVisibility(int visibility) {
        if(visibility == VISIBLE){
            postInvalidate();
        }
        super.setVisibility(visibility);
    }
}

```