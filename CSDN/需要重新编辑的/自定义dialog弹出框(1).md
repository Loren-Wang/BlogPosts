```
package com.daniujuntuan.android.view;

import android.app.Dialog;
import android.content.Context;
import android.graphics.drawable.AnimationDrawable;
import android.graphics.drawable.BitmapDrawable;
import android.support.v4.view.PagerAdapter;
import android.support.v4.view.ViewPager;
import android.text.TextWatcher;
import android.view.Display;
import android.view.Gravity;
import android.view.View;
import android.view.ViewGroup;
import android.view.WindowManager;
import android.widget.Adapter;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ImageView;
import android.widget.ListAdapter;
import android.widget.ListView;
import android.widget.TextView;

import com.daniujuntuan.android.R;
import com.lidroid.xutils.view.annotation.event.OnClick;
import com.nostra13.universalimageloader.core.ImageLoader;

import java.util.ArrayList;
import java.util.List;

/**
 * Created by Administrator on 2015/12/22 0022.
 */
public class MyDialog extends Dialog{
    private static ViewPager vgBigPhoto;
    private static AnimationDrawable rocketAnimation;
    private Context context = null;
    private static MyDialog customProgressDialog = null;
    private static float currentDistance=0;
    private static float lastDistance=-1;

    public MyDialog(Context context) {
        super(context);
        this.context = context;
    }

    public MyDialog(Context context, int themeResId) {
        super(context, themeResId);
    }

    protected MyDialog(Context context, boolean cancelable, OnCancelListener cancelListener) {
        super(context, cancelable, cancelListener);
    }

    public static MyDialog createDialog(Context context){
        customProgressDialog = new MyDialog(context, R.style.CustomProgressDialog1);
        customProgressDialog.setContentView(R.layout.progress_dialog);
        customProgressDialog.getWindow().getAttributes().gravity = Gravity.BOTTOM;
        customProgressDialog.setCanceledOnTouchOutside(false);
        customProgressDialog.setCancelable(false);

        ImageView rocketImage = (ImageView) customProgressDialog.findViewById(R.id.loading);
        rocketImage.setBackgroundResource(R.anim.loading_anim);
        rocketAnimation = (AnimationDrawable) rocketImage.getBackground();


        WindowManager m= (WindowManager) context.getSystemService(Context.WINDOW_SERVICE);
        Display d = m.getDefaultDisplay();  //为获取屏幕宽、高
        WindowManager.LayoutParams p = customProgressDialog.getWindow().getAttributes();  //获取对话框当前的参数值
        int pixelSize = (int) (context.getResources().getDimensionPixelSize(R.dimen.title_height)*1.35);
        p.height = (int) (d.getHeight()-pixelSize);
        p.width = (int) (d.getWidth() * 1.0);
        customProgressDialog.getWindow().setAttributes(p);
        return customProgressDialog;
    }

    @Override
    public void onWindowFocusChanged(boolean hasFocus) {
        if(rocketAnimation!=null) {
            rocketAnimation.start();
        }
        super.onWindowFocusChanged(hasFocus);
    }

    public static MyDialog createViewPagerBigPhotoDialog(final List<ImageView> list,Context context){
        customProgressDialog = new MyDialog(context,R.style.CustomViewPagerDialog);
        customProgressDialog.setContentView(R.layout.big_photo_show);
        customProgressDialog.getWindow().getAttributes().gravity = Gravity.FILL;
        customProgressDialog.setCanceledOnTouchOutside(false);
        customProgressDialog.setCancelable(false);

        final TextView tvImageNum = (TextView) customProgressDialog.findViewById(R.id.tvImageNum);
        ImageView imgBack= (ImageView) customProgressDialog.findViewById(R.id.imgBack);

            tvImageNum.setText("1/"+list.size());

        vgBigPhoto = (ViewPager)customProgressDialog.findViewById(R.id.vgBigPhoto);

        List<ImageView> imageViews = new ArrayList<ImageView>();
        for(int i=0;i<list.size();i++){
             ImageView imageView = new ImageView(context);
            BitmapDrawable drawable= (BitmapDrawable) list.get(i).getDrawable();//drawable转bitmap
            imageView.setImageBitmap(drawable.getBitmap());
            imageViews.add(imageView);
        }
        DialogViewPagerShowAdapter adapter = new DialogViewPagerShowAdapter(imageViews,context);
        vgBigPhoto.setAdapter(adapter);

        vgBigPhoto.addOnPageChangeListener(new ViewPager.OnPageChangeListener() {
            @Override
            public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {

            }

            @Override
            public void onPageSelected(int position) {
                tvImageNum.setText((++position) + "/" + list.size());
            }

            @Override
            public void onPageScrollStateChanged(int state) {

            }
        });
        imgBack.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                customProgressDialog.dismiss();
            }
        });


        return customProgressDialog;
    }

    public static MyDialog createListViewDialog(Context context){
        customProgressDialog = new MyDialog(context, R.style.CustomProgressDialog);
        customProgressDialog.setContentView(R.layout.list_confirm_dialog);
        customProgressDialog.getWindow().getAttributes().gravity = Gravity.CENTER;
        customProgressDialog.setCanceledOnTouchOutside(false);
        customProgressDialog.setCancelable(false);
        return customProgressDialog;
    }

    public static MyDialog createBigPhotoDialog(Context context){
        customProgressDialog = new MyDialog(context,R.style.CustomViewPagerDialog);
        customProgressDialog.setContentView(R.layout.gridview_big_photo);
        customProgressDialog.getWindow().getAttributes().gravity = Gravity.FILL;

        return customProgressDialog;
    }


    public static MyDialog createEditTextDialog(Context context,TextWatcher edittext,View.OnClickListener leftButton,View.OnClickListener rightButton){
        customProgressDialog = new MyDialog(context,R.style.CustomEdittextDialog1);
        customProgressDialog.setContentView(R.layout.dialog_edittext);
        ClearableEditText edtBuyerBidPrice= (ClearableEditText) customProgressDialog.findViewById(R.id.edtBuyerBidPrice);
        Button cancel_btn = (Button) customProgressDialog.findViewById(R.id.cancel_btn);
        Button confirm_btn = (Button) customProgressDialog.findViewById(R.id.confirm_btn);

        edtBuyerBidPrice.addTextChangedListener((TextWatcher) edittext);
        cancel_btn.setOnClickListener((View.OnClickListener) leftButton);
        confirm_btn.setOnClickListener((View.OnClickListener) rightButton);


        WindowManager m= (WindowManager) context.getSystemService(Context.WINDOW_SERVICE);
        Display d = m.getDefaultDisplay();  //为获取屏幕宽、高
        WindowManager.LayoutParams p = customProgressDialog.getWindow().getAttributes();  //获取对话框当前的参数值
        p.width = (int) (d.getWidth() * 0.7);
        customProgressDialog.getWindow().setAttributes(p);


        return customProgressDialog;
    }

    /**
     *
     * [Summary]
     *       setBigPhoto 大图片展示
     * @param path
     * @return
     *
     */
    public MyDialog setBigPhoto(String path){
        ImageView imgBigPhoto = (ImageView)customProgressDialog.findViewById(R.id.imgBigPhoto);

        if (imgBigPhoto != null){
            ImageLoader.getInstance().displayImage(path, imgBigPhoto);
        }

        imgBigPhoto.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                dismiss();
            }
        });

        return customProgressDialog;
    }



    /**
     *
     * [Summary]
     *       setBigPhoto 大图片展示
     * @return
     *
     */
    public MyDialog setViewPagerBigPhotoPosi(int posi){
        vgBigPhoto.setCurrentItem(posi);
        return customProgressDialog;
    }

    public MyDialog setListViewAdapter(Adapter adapter){
        ListView listView = (ListView)customProgressDialog.findViewById(R.id.lvList);
        listView.setAdapter((ListAdapter) adapter);
        return customProgressDialog;
    }

    /**
     *
     * [Summary]
     *       setBigPhoto 网络大图片展示
     * @param image
     * @return
     *
     */
    public MyDialog setBigPhoto(ImageView image){
//        ImageView imgBigPhoto = (ImageView)customProgressDialog.findViewById(R.id.imgBigPhoto);
//
//        if (imgBigPhoto != null){
//            imgBigPhoto.setImageDrawable(image.getDrawable());
//        }
//
//        imgBigPhoto.setOnClickListener(new View.OnClickListener() {
//            @Override
//            public void onClick(View v) {
//                dismiss();
//            }
//        });

        return customProgressDialog;
    }


    /**
     *
     * [Summary]
     *       setMessage 提示内容
     * @param strMessage
     * @return
     *
     */
    public MyDialog setMessage(String strMessage){
        TextView tvMsg = (TextView)customProgressDialog.findViewById(R.id.tvHintMessage);

        if (tvMsg != null){
            tvMsg.setText(strMessage);
        }

        return customProgressDialog;
    }

    static class DialogViewPagerShowAdapter extends PagerAdapter {
        private Context context;
        public DialogViewPagerShowAdapter(Context context){
            this.context=context;
        }
        private List<ImageView> list = new ArrayList<ImageView>();
        private boolean noClick=false;


        public DialogViewPagerShowAdapter(List<ImageView> list,Context context) {
            this.list = list;
            this.context=context;
        }

        @Override
        public int getCount() {
            return list!=null?list.size():0;
        }

        @Override
        public boolean isViewFromObject(View view, Object object) {
            return view==object;
        }

        @Override
        public void destroyItem(ViewGroup container, int position, Object object) {
            container.removeView(list.get(position));
        }

        @Override
        public Object instantiateItem(ViewGroup container, final int position) {
            container.addView(list.get(position));
            return list.get(position);
        }

    }

    @Override
    public void dismiss() {


        try {
            Thread.sleep(10);
            super.dismiss();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }


    }

}

```