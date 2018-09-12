
引用了一小部分其他人的
```
package com.daniujuntuan.android.adapter;

import android.content.Context;
import android.content.DialogInterface;
import android.os.Bundle;
import android.support.v4.view.PagerAdapter;
import android.view.KeyEvent;
import android.view.View;
import android.view.ViewGroup;
import android.view.ViewParent;
import android.widget.ImageView;

import com.daniujuntuan.android.AppCommon;
import com.daniujuntuan.android.activity.common.WebViewShowActivity;
import com.daniujuntuan.android.dto.app_home_page.AppHomePageDataCarouselListDto;
import com.daniujuntuan.android.utils.ActivityUtils;
import com.daniujuntuan.android.view.MyDialog;

import java.util.ArrayList;
import java.util.List;

/**
 * Created by Administrator on 2015/12/24 0024.
 */
public class ViewPagerShowAdapter extends PagerAdapter {
    private Context context;
    public ViewPagerShowAdapter(Context context){
        this.context=context;
    }
    private List<ImageView> list = new ArrayList<ImageView>();
    List<AppHomePageDataCarouselListDto> carouselLis = new ArrayList<AppHomePageDataCarouselListDto>();
    private boolean noClick=false;
    private boolean clickAppHomePageJump=false;


    public ViewPagerShowAdapter(List<ImageView> list,Context context) {
        this.list = list;
        this.context=context;
    }

    public ViewPagerShowAdapter(Context context,List<ImageView> list,List<AppHomePageDataCarouselListDto> carouselLis) {
        this.list = list;
        this.carouselLis=carouselLis;
        this.context=context;
        clickAppHomePageJump = true;//用来标识是否是首页点击跳转
    }

    @Override
    public int getCount() {
        if(list!=null&&list.size()<3){
            return list.size();
        }
        return list!=null?list.size()==1?1:Integer.MAX_VALUE/100:0;//先判断list是否为空，在判断list的大小是否为1
    }
//    list!=null?list.size():0;//
    @Override
    public boolean isViewFromObject(View view, Object object) {
        return view==object;
    }

    @Override
    public void destroyItem(ViewGroup container, int position, Object object) {
//        int posi = position%list.size();
//        container.removeView(list.get(posi));
    }

    @Override
    public Object instantiateItem(ViewGroup container, int position) {
        //对ViewPager页号求模取出View列表中要显示的项
        position %= list.size();
        if (position<0){
            position = list.size()+position;
        }
        ImageView view = list.get(position);
        //如果View已经在之前添加到了一个父组件，则必须先remove，否则会抛出IllegalStateException。
        ViewParent vp =view.getParent();
        if (vp!=null){
            ViewGroup parent = (ViewGroup)vp;
            parent.removeView(view);
        }
        container.addView(view);
        //add listeners here if necessary

        if(!clickAppHomePageJump&&!noClick) {
            final int finalPosition = position;
            list.get(finalPosition).setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {

                    MyDialog myDialog = MyDialog.createViewPagerBigPhotoDialog(list, context);
                    myDialog.setViewPagerBigPhotoPosi(finalPosition);
                    myDialog.show();
                    myDialog.setOnKeyListener(new DialogInterface.OnKeyListener() {
                        @Override
                        public boolean onKey(DialogInterface dialog, int keyCode, KeyEvent event) {
                            dialog.dismiss();
                            return false;
                        }
                    });
                }
            });
        }
        if(clickAppHomePageJump){
            final int finalPosition1 = position;
            list.get(finalPosition1).setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    if(carouselLis.get(finalPosition1).getWebUrl()!=null||!carouselLis.get(finalPosition1).getWebUrl().isEmpty()||!"".equals(carouselLis.get(finalPosition1).getWebUrl())) {
                        Bundle bundle = new Bundle();
                        bundle.putString(AppCommon.WEBVIEW_URL, carouselLis.get(finalPosition1).getWebUrl());
                        bundle.putString(AppCommon.WEBVIEW_TITLT,carouselLis.get(finalPosition1).getTitle());
                        bundle.putString(AppCommon.PAGE_TYPE, AppCommon.PAGE_TYPE_HOME_PAGE_FRAGMENT);
                        ActivityUtils.jump(context, WebViewShowActivity.class, bundle);
                    }
                }
            });
        }

        return view;


//        final int posi = position%list.size();
//        container.addView(list.get(posi));
//        if(!clickAppHomePageJump&&!noClick) {
//            list.get(posi).setOnClickListener(new View.OnClickListener() {
//                @Override
//                public void onClick(View v) {
//
//                    MyDialog myDialog = MyDialog.createViewPagerBigPhotoDialog(list, context);
//                    myDialog.setViewPagerBigPhotoPosi(posi);
//                    myDialog.show();
//                    myDialog.setOnKeyListener(new DialogInterface.OnKeyListener() {
//                        @Override
//                        public boolean onKey(DialogInterface dialog, int keyCode, KeyEvent event) {
//                            dialog.dismiss();
//                            return false;
//                        }
//                    });
//                }
//            });
//        }
//        if(clickAppHomePageJump){
//            list.get(posi).setOnClickListener(new View.OnClickListener() {
//                @Override
//                public void onClick(View v) {
//                    if(carouselLis.get(posi).getWebUrl()!=null||!carouselLis.get(posi).getWebUrl().isEmpty()||!"".equals(carouselLis.get(posi).getWebUrl())) {
//                        Bundle bundle = new Bundle();
//                        bundle.putString(AppCommon.WEBVIEW_URL, carouselLis.get(posi).getWebUrl());
//                        bundle.putString(AppCommon.WEBVIEW_TITLT,carouselLis.get(posi).getTitle());
//                        bundle.putString(AppCommon.PAGE_TYPE, AppCommon.PAGE_TYPE_HOME_PAGE_FRAGMENT);
//                        ActivityUtils.jump(context, WebViewShowActivity.class,bundle);
//                    }
//                }
//            });
//        }
//        return list.get(posi);
    }


    public  void setNoClick(){
        noClick = true;
    }
    
}

```