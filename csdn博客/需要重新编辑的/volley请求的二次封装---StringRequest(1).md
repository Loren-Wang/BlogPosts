！！！！！！！！！！！！！！！！原创文章，转载请注明出处！！！！！！！！！！！！！！！！



 volley请求的二次封装，关于StringRequest的，没有写很多，也没有验证，不过以前写过类似的，能够直接运行，这个不出意外的话也可以。

本人的二次封装不知道是否合理，不过这是我很喜欢的类型，封装到一个类里，可以随意修改，后续还会有其他的关于volley的二次封装，不过不在一个类里，但是却可以放在一个类里。

后续还会继续优化！！！！

①.2016.2.3：将请求变成了单例模式，同样没有测试

```
package com.daniujuntuan.android.utils;

import android.content.Context;
import android.os.Handler;
import android.os.Message;

import com.android.volley.AuthFailureError;
import com.android.volley.NetworkResponse;
import com.android.volley.ParseError;
import com.android.volley.Request;
import com.android.volley.RequestQueue;
import com.android.volley.Response;
import com.android.volley.VolleyError;
import com.android.volley.toolbox.HttpHeaderParser;
import com.android.volley.toolbox.StringRequest;
import com.android.volley.toolbox.Volley;

import org.json.JSONException;
import org.json.JSONObject;

import java.io.UnsupportedEncodingException;
import java.util.Map;

/**
 * Created by Administrator on 2016/1/30 0030.
 */
public class VolleyStringRequestPacking {
    private static RequestQueue mQueue;
    public static final int REQUEST_FAIL=-1;//网络请求失败
    public static final int REQUEST_Exceptions=-2;//网络请求异常
    public static final String LOADING_FAIL="雾霾太大，您的网络已迷失在三环外，请检查网络连接";
    final String loadint_Exceptions="请求的标记不能为"+ REQUEST_FAIL+"或者"+REQUEST_Exceptions;
    final String responseResuleCharacterEncodingDefault="UTF-8";
    private static Context context;
    private static StringRequest request;//当前正在执行的请求
    private static  VolleyStringRequestPacking volleyStringRequestPacking;

    private VolleyStringRequestPacking(Context context){
        mQueue = Volley.newRequestQueue(context);
    }
    public static VolleyStringRequestPacking getInstance(Context context){
        if(volleyStringRequestPacking==null){
            volleyStringRequestPacking= new VolleyStringRequestPacking(context);
        }
        return volleyStringRequestPacking;
    }


    /**
     *   使用get请求获得数据，返回结果使用自定义的默认“utf-8”编码
     * @param context
     * @param requestPath  请求地址
     * @param receiveResuleHandler 接收请求结果的handler
     * @param receiveSignNum  接收请求结果的标志数
     */
    public void loadingGetNetData_StringRequest(Context context,String requestPath, final Handler receiveResuleHandler, final int receiveSignNum){
        this.context=context;
        sendStringRequest(requestPath, Request.Method.GET, receiveResuleHandler, receiveSignNum,null,null,null);
    }

    /**
     *   使用get请求获得数据，返回结果使用所传递的编码
     * @param context
     * @param requestPath  请求地址
     * @param receiveResuleHandler 接收请求结果的handler
     * @param receiveSignNum  接收请求结果的标志数
     * @param responseResuleCharacterEncoding 返回的结果集，暂时用来设置返回结果字符串的编码格式,例如"Utf-8"等
     */
    public void loadingGetNetData_StringRequest(Context context,String requestPath,
                                                final Handler receiveResuleHandler, final int receiveSignNum,String responseResuleCharacterEncoding){
        this.context=context;
        sendStringRequest(requestPath, Request.Method.GET, receiveResuleHandler, receiveSignNum, null, null, responseResuleCharacterEncoding);
    }

    /**
     *   使用post请求获得数据，返回结果使用自定义的默认“utf-8”编码
     * @param context
     * @param requestPath  请求地址
     * @param receiveResuleHandler 接收请求结果的handler
     * @param receiveSignNum  接收请求结果的标志数
     * @param paramsMap post请求的参数集合
     */
    public void loadingPostNetData_StringRequest(Context context,String requestPath, final Handler receiveResuleHandler, final int receiveSignNum,
                                                 Map<String,String> paramsMap){
        this.context=context;
        sendStringRequest(requestPath, Request.Method.POST, receiveResuleHandler, receiveSignNum,paramsMap,null,null);
    }

    /**
     *   使用get请求获得数据，返回结果使用所传递的编码
     * @param context
     * @param requestPath  请求地址
     * @param receiveResuleHandler 接收请求结果的handler
     * @param receiveSignNum  接收请求结果的标志数
     * @param paramsMap post请求的参数集合
     * @param responseResuleCharacterEncoding 返回的结果集，暂时用来设置返回结果字符串的编码格式,例如"Utf-8"等
     */
    public void loadingPostNetData_StringRequest(Context context,String requestPath, final Handler receiveResuleHandler, final int receiveSignNum,
                                                 Map<String,String> paramsMap,String responseResuleCharacterEncoding){
        this.context=context;
        sendStringRequest(requestPath, Request.Method.POST, receiveResuleHandler, receiveSignNum, paramsMap, null, responseResuleCharacterEncoding);
    }



    /**
     *    vvolley发送网络请求
     * @param requestPath  请求地址
     * @param requestType 请求类型，get或post
     * @param receiveResuleHandler 接收请求结果的handler
     * @param receiveSignNum  接收请求结果的标志数
     * @param paramsMap post请求的参数集合
     * @param headersMap 请求头集合
     * @param responseResuleCharacterEncoding 返回的结果集，暂时用来设置返回结果字符串的编码格式,例如"Utf-8"等
     */
    private void sendStringRequest(final String requestPath,int requestType, final Handler receiveResuleHandler, final int receiveSignNum,
                             final Map<String, String> paramsMap, final Map<String, String> headersMap, final String responseResuleCharacterEncoding) {
        final Message message=Message.obtain();
        if(receiveSignNum== REQUEST_Exceptions||receiveSignNum==REQUEST_FAIL){
            message.what=REQUEST_Exceptions;
            message.obj=loadint_Exceptions;
            receiveResuleHandler.sendMessage(message);
            return;
        }
        request = new StringRequest(requestType, requestPath, new Response.Listener<String>() {
            @Override
            public void onResponse(String s) {
                message.what=receiveSignNum;
                message.obj=s;
                receiveResuleHandler.sendMessage(message);
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError volleyError) {
                message.what=REQUEST_FAIL;
                message.obj=volleyError.getMessage();
                receiveResuleHandler.sendMessage(message);
            }
        }){
            @Override
            protected Map<String, String> getParams() throws AuthFailureError {
                if(paramsMap==null) {
                    return super.getParams();
                }else {
                    return paramsMap;
                }
            }

            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                if(headersMap==null) {
                    return super.getHeaders();
                }else {
                    return headersMap;
                }
            }

            @Override
            protected Response<String> parseNetworkResponse(NetworkResponse response) {
                try {
                    String jsonString = new String(response.data, responseResuleCharacterEncoding==null?responseResuleCharacterEncodingDefault:responseResuleCharacterEncoding);
                    return Response.success(new JSONObject(jsonString).toString(), HttpHeaderParser.parseCacheHeaders(response));
                } catch (UnsupportedEncodingException e) {
                    return Response.error(new ParseError(e));
                } catch (JSONException je) {
                    return Response.error(new ParseError(je));
                }
            }

        };
        mQueue.add(request);
    }

    /**
     * 结束当前请求
     */
    private static void stopCurrentRequest(){
       request.cancel();
    }

    /**
     * 请求所有请求
     */
    private static void stopAllRequest(){
        mQueue.cancelAll(context);
    }
}

```

