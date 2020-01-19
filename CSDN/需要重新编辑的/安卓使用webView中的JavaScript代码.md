在安卓程序中调用webview是一件很简单的事，但是想要获得webview的代码却是比较麻烦，
首先要进行的一步操作就是
 
 第一步：

```
WebSettings settings = wvIntroduce.getSettings();
settings.setJavaScriptEnabled(true);//使webview能够支持JavaScript代码
```

第二步：
创建一个类作为借口，然后让html代码调用该接口，使得将数据 传回安卓程序中（注意 @JavascriptInterface必须得有）
   

```
 class ShareInterface{
        @JavascriptInterface
        public void shareEvent(String content,String image,String title,String url){
            ToolUtils.getInstance(context).popWeChatShareing( LayoutInflater.from(context).inflate(R.layout.poping_layout, null),findViewById(R.id.popStart),
                    content,image,R.drawable.ic_launcher,title,url);
        }
    }
```

第三步：
创建ShareInterface对象同时将该对象以及html代码的要调用的对象名添加到webview当中

```
 ShareInterface shareInterface = new ShareInterface();
 wvIntroduce.addJavascriptInterface(shareInterface, "android_callBack");//"android_callBack"为html代码要调用的对象名
 
```

第四步：
在html代码中的function函数当中使用 `android_callBack.shareEvent(content,image,title,url);`方法将值回调给安卓程序




样例代码

```
package com.daniujuntuan.android.activity.common;

import android.app.Activity;
import android.content.Context;
import android.os.Bundle;
import android.view.KeyEvent;
import android.view.LayoutInflater;
import android.view.View;
import android.webkit.JavascriptInterface;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.webkit.WebViewClient;
import android.widget.ImageView;
import android.widget.TextView;

import com.daniujuntuan.android.AppCommon;
import com.daniujuntuan.android.R;
import com.daniujuntuan.android.activity.MainActivity;
import com.daniujuntuan.android.activity.anzahlungsgarantie_f.AnzahlungsgarantieFActivity;
import com.daniujuntuan.android.activity.help_look.HelpLookActivity;
import com.daniujuntuan.android.activity.help_look.HelpLookBelowOrderActivity;
import com.daniujuntuan.android.activity.help_look.HelpLookOrderDetailActivity;
import com.daniujuntuan.android.activity.user.log_regist.RegisterActivity;
import com.daniujuntuan.android.utils.ActivityUtils;
import com.daniujuntuan.android.utils.ToolUtils;
import com.daniujuntuan.android.wxapi.WechatLoginBindingMobileActivity;

public class WebViewShowActivity extends Activity {

    private WebView wvIntroduce;
    private TextView tvTitle,tvScreen;
    private ImageView imgBack;
    private Context context;
    private ImageView imgTitleRightImage;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_webview_show);
        context = this;
        wvIntroduce = (WebView) findViewById(R.id.wvIntroduce);
        tvScreen = (TextView) findViewById(R.id.tvScreen);
        tvTitle = (TextView) findViewById(R.id.tvTitle);
        imgBack = (ImageView) findViewById(R.id.imgBack);
        imgTitleRightImage = (ImageView) findViewById(R.id.imgTitleRightImage);
        imgTitleRightImage.setVisibility(View.VISIBLE);
        tvScreen.setVisibility(View.GONE);

        imgBack.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (wvIntroduce.canGoBack()) {
                    wvIntroduce.goBack();
                    return;
                }
                if (AppCommon.ANZAHLUNGSGARANTIEF_INTRODUCE.equals(getIntent().getExtras().getString(AppCommon.WEBVIEW_URL))) {
                    ActivityUtils.jump(context, AnzahlungsgarantieFActivity.class, getIntent().getExtras());
                    finish();
                } else if (AppCommon.ANZAHLUNGSGARANTIEF_RULES.equals(getIntent().getExtras().getString(AppCommon.WEBVIEW_URL))) {
                    ActivityUtils.jump(context, AnzahlungsgarantieFActivity.class, getIntent().getExtras());
                    finish();
                } else if (AppCommon.HELP_LOOK_ORDER_REPORT.equals(getIntent().getExtras().getString(AppCommon.WEBVIEW_URL)) ||
                        (AppCommon.HELP_LOOK_ORDER_INTRODUCE.equals(getIntent().getExtras().getString(AppCommon.WEBVIEW_URL))
                                && AppCommon.HELP_LOOK_ORDER_INTRODUCE_ORDER_DETAIL.equals(getIntent().getExtras().getString(AppCommon.HELP_LOOK_ORDER_INTRODUCE_WHO)))) {
                    ActivityUtils.jump(context, HelpLookOrderDetailActivity.class, getIntent().getExtras());
                    finish();
                } else if (AppCommon.HELP_LOOK_ORDER_INTRODUCE.equals(getIntent().getExtras().getString(AppCommon.WEBVIEW_URL))
                        && AppCommon.HELP_LOOK_ORDER_INTRODUCE_BELOW_ORDER.equals(getIntent().getExtras().getString(AppCommon.HELP_LOOK_ORDER_INTRODUCE_WHO))
                        || (AppCommon.HELP_LOOK_BELOW_ORDER_MUST_KNOW.equals(getIntent().getExtras().getString(AppCommon.WEBVIEW_URL)))) {
                    ActivityUtils.jump(context, HelpLookBelowOrderActivity.class);
                    finish();
                } else if (AppCommon.HELP_LOOK_INTRODUCE.equals(getIntent().getExtras().getString(AppCommon.WEBVIEW_URL))) {
                    ActivityUtils.jump(context, HelpLookActivity.class);
                    finish();
                } else if (AppCommon.REGIST_SERVICE_AGREEMENT_REGIST.equals(getIntent().getExtras().getString(AppCommon.REGIST_SERVICE_AGREEMENT_WHO))) {
                    ActivityUtils.jump(context, RegisterActivity.class, getIntent().getExtras());
                    finish();
                } else if (AppCommon.REGIST_SERVICE_AGREEMENT_BINDING.equals(getIntent().getExtras().getString(AppCommon.REGIST_SERVICE_AGREEMENT_WHO))) {
                    ActivityUtils.jump(context, WechatLoginBindingMobileActivity.class, getIntent().getExtras());
                    finish();
                } else if (AppCommon.PAGE_TYPE_HOME_PAGE_FRAGMENT.equals(getIntent().getExtras().getString(AppCommon.PAGE_TYPE))) {
                    Bundle bundle = new Bundle();
                    bundle.putString(AppCommon.MAIN_PAGE_TYPE, AppCommon.MAIN_PAGE_HOME_PAGE);
                    ActivityUtils.jump(context, MainActivity.class, bundle);
                    finish();
                }
            }
        });
        tvScreen.setVisibility(View.GONE);
        String pathUrl="";

        Bundle bundle=getIntent().getExtras();
        if(bundle!=null) {
            tvTitle.setText(bundle.getString(AppCommon.WEBVIEW_TITLT));
            pathUrl = bundle.getString(AppCommon.WEBVIEW_URL) + "?from=app";
            if (AppCommon.ANZAHLUNGSGARANTIEF_INTRODUCE.equals(getIntent().getExtras().getString(AppCommon.WEBVIEW_URL))) {
                pathUrl = bundle.getString(AppCommon.WEBVIEW_URL) + "?from=app";
            }
            if (AppCommon.ANZAHLUNGSGARANTIEF_RULES.equals(getIntent().getExtras().getString(AppCommon.WEBVIEW_URL))) {
                pathUrl = bundle.getString(AppCommon.WEBVIEW_URL) + "?from=app";
            }
            if (AppCommon.HELP_LOOK_ORDER_REPORT.equals(getIntent().getExtras().getString(AppCommon.WEBVIEW_URL))) {
                pathUrl = bundle.getString(AppCommon.WEBVIEW_URL) + bundle.getString(AppCommon.HELP_LOOK_ORDER_ORDERID) + "?from=app";
            }
            if (AppCommon.HELP_LOOK_ORDER_INTRODUCE.equals(getIntent().getExtras().getString(AppCommon.WEBVIEW_URL))) {
                pathUrl = bundle.getString(AppCommon.WEBVIEW_URL) + "?from=app";
            }
            if (AppCommon.REGIST_SERVICE_AGREEMENT.equals(getIntent().getExtras().getString(AppCommon.WEBVIEW_URL))) {
                pathUrl = bundle.getString(AppCommon.WEBVIEW_URL) + "?from=app";
            }
            if (AppCommon.PAGE_TYPE_HOME_PAGE_FRAGMENT.equals(getIntent().getExtras().getString(AppCommon.PAGE_TYPE))) {
                pathUrl = bundle.getString(AppCommon.WEBVIEW_URL) + "?from=app";
            }
            loadingWebView(pathUrl);
        }
    }

    private void loadingWebView(String pathUrl) {
        ShareInterface shareInterface = new ShareInterface();
        WebSettings settings = wvIntroduce.getSettings();
        settings.setJavaScriptEnabled(true);
        wvIntroduce.loadUrl(pathUrl);
        wvIntroduce.addJavascriptInterface(shareInterface, "android_callBack");
        wvIntroduce.setWebViewClient(new WebViewClient() {
            @Override
            public boolean shouldOverrideUrlLoading(WebView view, String url) {
                String urlPath = url + "?from=app";
                wvIntroduce.loadUrl(urlPath);
                return true;
            }

        });
    }

    class ShareInterface{
        @JavascriptInterface
        public void shareEvent(String content,String image,String title,String url){
            ToolUtils.getInstance(context).popWeChatShareing( LayoutInflater.from(context).inflate(R.layout.poping_layout, null),findViewById(R.id.popStart),
                    content,image,R.drawable.ic_launcher,title,url);
//            popWeChatShareing(content, image, title, url);
        }
    }

//    //微信分享选择弹出框
//    private void popWeChatShareing(String content, final String image,String title,String url) {
//        View view = LayoutInflater.from(this).inflate(R.layout.poping_layout, null);
//        final ImageView imgWxFriend = (ImageView) view.findViewById(R.id.imgWxFriend);//分享给朋友
//        ImageView imgCircleOfFriends = (ImageView) view.findViewById(R.id.imgCircleOfFriends);//分享的朋友圈
//        TextView tvCancle = (TextView) view.findViewById(R.id.tvCancle);
//        final PopupWindow window=popBackgroundAnim(view);
//
//
//        //微信分享
//        WXWebpageObject webpage = new WXWebpageObject(); //初始化一个WXWebObject对象
//        //分享url
//        webpage.webpageUrl = url;
//        final WXMediaMessage msg = new WXMediaMessage(webpage);//用WXTextObject实例化一个WXMediaMessage对象
//        //分享的标题
//        msg.title=title;
//        //分享的内容
//        msg.description=content;
//        //分享的图片
//        Bitmap thumb = BitmapUtil.drawableToBitmap(getResources().getDrawable(R.drawable.ic_launcher));
//        msg.setThumbImage(thumb);//设置默认，防止图片未缓存到时分享
//
//
//        window.showAsDropDown(view);
//
//        tvCancle.setOnClickListener(new View.OnClickListener() {
//            @Override
//            public void onClick(View v) {
//                window.dismiss();
//            }
//        });
//
//        imgWxFriend.setOnClickListener(new View.OnClickListener() {
//            @Override
//            public void onClick(View v) {
//
//                final MyDialog myDialog =MyDialog.createDialog(context);
//                myDialog.setMessage("请稍后...");
//                myDialog.setCancelable(false);
//                myDialog.show();
//
//
//                Handler handlerBitmap = new Handler() {
//                    @Override
//                    public void handleMessage(Message message) {
//                        super.handleMessage(message);
//                        Bitmap bitmap = (Bitmap) message.obj;
//                        if(bitmap==null){
//                            shareFriend(msg, window, myDialog);
//                            return;
//                        }
//                        bitmap = PhotoDisposeUtils.compressWeChatImage(bitmap, 10);
//                        msg.setThumbImage(bitmap);
//                        shareFriend(msg, window, myDialog);
//                    }
//                };
//                VolleyImageRequestPacking.getInstance(context).loadingNetImageToBitmap(image, handlerBitmap, 0, 0, 0, Bitmap.Config.RGB_565);
//
//            }
//        });
//
//        imgCircleOfFriends.setOnClickListener(new View.OnClickListener() {
//            @Override
//            public void onClick(View v) {
//
//
//                final MyDialog myDialog =MyDialog.createDialog(context);
//                myDialog.setMessage("请稍后...");
//                myDialog.setCancelable(false);
//                myDialog.show();
//
//                Handler handlerBitmap = new Handler() {
//                    @Override
//                    public void handleMessage(Message message) {
//                        super.handleMessage(message);
//                        Bitmap bitmap = (Bitmap) message.obj;
//                        if(bitmap==null){
//                            shareCircleofFriends(msg, window, myDialog);
//                            return;
//                        }
//                        bitmap = PhotoDisposeUtils.compressWeChatImage(bitmap, 10);
//                        msg.setThumbImage(bitmap);
//                        shareCircleofFriends(msg, window, myDialog);
//                    }
//                };
//                VolleyImageRequestPacking.getInstance(context).loadingNetImageToBitmap(image,handlerBitmap,0,0,0, Bitmap.Config.RGB_565);
//
////                ImageRequest request=new ImageRequest(photoList.get(0), new Response.Listener<Bitmap>() {
////                    @Override
////                    public void onResponse(Bitmap bitmap) {
////                        if(bitmap==null){
////                            shareCircleofFriends(msg, window, myDialog);
////                            return;
////                        }
////                        bitmap = PhotoDisposeUtils.compressWeChatImage(bitmap, 10);
////                        msg.setThumbImage(bitmap);
////                        shareCircleofFriends(msg, window, myDialog);
////                    }
////                }, 0, 0, Bitmap.Config.RGB_565, new Response.ErrorListener() {
////                    @Override
////                    public void onErrorResponse(VolleyError volleyError) {
////                        Bitmap thumb = BitmapUtil.drawableToBitmap(getResources().getDrawable(R.drawable.iconfont_car));
////                        msg.setThumbImage(thumb);
////                        shareCircleofFriends(msg, window, myDialog);
////                    }
////                });
////                MyApplication.getHttpQueue().add(request);
//            }
//        });
//
//    }
//
//    //弹出框背景以及动画
//    private PopupWindow popBackgroundAnim(View view) {
//        // 下面是两种方法得到宽度和高度 getWindow().getDecorView().getWidth()
//        PopupWindow window = new PopupWindow(view,
//                WindowManager.LayoutParams.MATCH_PARENT,
//                WindowManager.LayoutParams.WRAP_CONTENT);
//
//        // 设置popWindow弹出窗体可点击，这句话必须添加，并且是true
//        window.setFocusable(true);
//
//        // 实例化一个ColorDrawable颜色为半透明
//        ColorDrawable dw = new ColorDrawable(0xb0000000);
//        window.setBackgroundDrawable(dw);
//
////        // 设置popWindow的显示和消失动画
////        window.setAnimationStyle(R.style.mypopwindow_anim_style);
//        // 在底部显示
//        window.showAtLocation(findViewById(R.id.popStart), Gravity.BOTTOM, 0, 0);
//        return window;
//    }
//
//    //    分享到朋友
//    private void shareFriend(WXMediaMessage msg,PopupWindow window,MyDialog myDialog){
//        boolean sIsWXAppInstalledAndSupported = MyApplication.getApi().isWXAppInstalled()
//                && MyApplication.getApi().isWXAppSupportAPI();
//        if(sIsWXAppInstalledAndSupported) {
//            SendMessageToWX.Req req = new SendMessageToWX.Req();//        构造一个rep
//            req.message = msg;
//            MyApplication.getApi().sendReq(req);
//            window.dismiss();
//            myDialog.dismiss();
//        }else {
//            DialogUtils.showToastShort(context, "您的手机未安装微信！");
//            myDialog.dismiss();
//        }
//    }
//
//    private void shareCircleofFriends(WXMediaMessage msg,PopupWindow window,MyDialog myDialog){
//        boolean sIsWXAppInstalledAndSupported = MyApplication.getApi().isWXAppInstalled()
//                && MyApplication.getApi().isWXAppSupportAPI();
//        if(sIsWXAppInstalledAndSupported) {
//            DialogUtils.showToastShort(context, "请稍候...");
//            SendMessageToWX.Req req = new SendMessageToWX.Req();//        构造一个rep
//            req.scene = SendMessageToWX.Req.WXSceneTimeline;
//            req.transaction = String.valueOf(System.currentTimeMillis());//transaction用来标记唯一标识请求
//            req.message = msg;
//            MyApplication.getApi().sendReq(req);
//            window.dismiss();
//            myDialog.dismiss();
//        }else {
//            DialogUtils.showToastShort(context, "您的手机未安装微信！");
//            myDialog.dismiss();
//        }
//    }
//


    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        if(keyCode==KeyEvent.KEYCODE_BACK&&wvIntroduce.canGoBack()){
            wvIntroduce.goBack();
            return true;
        }
        return super.onKeyDown(keyCode, event);
    }

    @Override
    public void onBackPressed() {
        imgBack.performClick();
        super.onBackPressed();
    }
}
```

