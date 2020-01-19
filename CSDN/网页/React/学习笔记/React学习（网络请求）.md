&nbsp;&nbsp;一开始在弄网络请求的时候我还以为有很多东西要弄，但是发现其实React的网络请求也就那么几种，首先是自带的**fetch**请求，这种最基础也不用特意安装；其次是和vue相同的**axios**，这玩意很通用啊，再者还有专门的**react-axios**，不过我并没有去研究或者看这个，因为之前用过vue，所以对于axios比较熟悉。

&nbsp;&nbsp;这篇文章主要介绍的就是axios，但是要注意的是和vue不同点就是在初始化的调用方面的处理。

**ps：vue是在初始化之后将初始化的变量赋值给vue的自定义全局变量当中，但是React是直接调用的，同时如果自定义初始化则不能调用axios.create()方法，否则会导致拦截器失效。**



#### 1、安装axios

执行npm命令：`npm isntall axios --save`

#### 2、初始化基础请求

```
import netWorkManager from 'axios'
import logInfo from "../common/Log";
//基础api
const BASE_URL = "http://***************/debug";

//超时时间
const TIMEOUT = 5000;
//设置基础地址
netWorkManager.defaults.baseURL = BASE_URL;
//设置超时时间
netWorkManager.defaults.timeout = TIMEOUT;
```

#### 3、初始化拦截器

```
//请求拦截
logInfo("netOptions:", "初始化请求拦截");
netWorkManager.interceptors.request.use(function (requestConfig) {
    logInfo("netOptions:", "请求拦截开始");
    logInfo("netOptions:", "请求base地址---" + requestConfig.baseURL);
    logInfo("netOptions:", "请求地址---" + requestConfig.url);
    logInfo("netOptions:", "请求参数---" + JSON.stringify(requestConfig.data));
    logInfo("netOptions:", "请求拦截结束");
    return requestConfig;
}, function (error) {

});
//响应拦截
logInfo("netOptions:", "初始化响应拦截");
netWorkManager.interceptors.response.use(response => {
    logInfo("netOptions:", "响应拦截开始");
    logInfo("netOptions:", "响应拦截获取响应code---" + response.data.stateCode);
    if (response.data.stateCode === 200) {
        logInfo("netOptions:", "响应拦截结束");
        return response.data.data
    } else {
        logInfo("netOptions:", "响应拦截结束");
        return response
    }
}, error => {

});

```

#### 4、发起请求

```
/**
 * 登录请求，
 * @param values 登录参数
 */
export function loginApi(values) {
    logInfo("netOptions:", "发起登录请求")
    netWorkManager.post(getRequestUrl(API_LIST.loginUrl), values)
        .then(response => {
            logInfo("netOptions:", "登录响应数据---" + JSON.stringify(response));
        }).catch(error => {
        logInfo("netOptions:", "登录异常数据---" + error);
    })
}
```



# 全部代码如下

```
import netWorkManager from 'axios'
import logInfo from "../common/Log";


//基础api
const BASE_URL = "http://***********************/debug";

//超时时间
const TIMEOUT = 5000;
//设置基础地址
netWorkManager.defaults.baseURL = BASE_URL;
//设置超时时间
netWorkManager.defaults.timeout = TIMEOUT;
//请求拦截
logInfo("netOptions:", "初始化请求拦截");
netWorkManager.interceptors.request.use(function (requestConfig) {
    logInfo("netOptions:", "请求拦截开始");
    logInfo("netOptions:", "请求base地址---" + requestConfig.baseURL);
    logInfo("netOptions:", "请求地址---" + requestConfig.url);
    logInfo("netOptions:", "请求参数---" + JSON.stringify(requestConfig.data));
    logInfo("netOptions:", "请求拦截结束");
    return requestConfig;
}, function (error) {

});
//响应拦截
logInfo("netOptions:", "初始化响应拦截");
netWorkManager.interceptors.response.use(response => {
    logInfo("netOptions:", "响应拦截开始");
    logInfo("netOptions:", "响应拦截获取响应code---" + response.data.stateCode);
    if (response.data.stateCode === 200) {
        logInfo("netOptions:", "响应拦截结束");
        return response.data.data
    } else {
        logInfo("netOptions:", "响应拦截结束");
        return response
    }
}, error => {

});

//api列表
const API_LIST = {
    loginUrl: "/user/login"
};

/**
 * 获取请求数据
 * @param url 请求参数
 * @return {*} 请求地址
 */
function getRequestUrl(url) {
    return url
}


/**
 * 登录请求，
 * @param values 登录参数
 */
export function loginApi(values) {
    logInfo("netOptions:", "发起登录请求")
    netWorkManager.post(getRequestUrl(API_LIST.loginUrl), values)
        .then(response => {
            logInfo("netOptions:", "登录响应数据---" + JSON.stringify(response));
        }).catch(error => {
        logInfo("netOptions:", "登录异常数据---" + error);
    })
}

```






















