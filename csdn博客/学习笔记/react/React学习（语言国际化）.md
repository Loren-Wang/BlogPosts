&nbsp;&nbsp;所谓的语言国际化就是讲网页内部的所有文本显示的内容做处理，使其根据不同的语言环境显示不同文案的功能，当然了，这个并不能对图片做处理，仅能对文本内容做处理，当然了，你也可以使用文本拼接的方式读取指定不同文件夹下的图片内容。

###### 参考文章：[https://blog.csdn.net/hfhwfw161226/article/details/81178515](https://blog.csdn.net/hfhwfw161226/article/details/81178515)

[https://www.npmjs.com/package/react-intl-universal](https://www.npmjs.com/package/react-intl-universal)

#### 1、安装插件

&nbsp;&nbsp;React使用其他第三方插件的话必须要安装进入，除非是单js文件使用html的标签引入也可以，安装命令如下：

```
npm install react-intl-universal --save
```

#### 2、添加语言包

&nbsp;&nbsp;这个语言包可是很重要的，因为所有的文案内容都在当中。**这个语言包要注意很重要的一点，那就是”内部json数据的层级仅为一层，也就是只能有一个json实体对象，不能有任何子对象“**。

 &nbsp;&nbsp;添加路径随意，起名随意，这方面没有硬性要求，只要最后使用是能寻址到就好。

#### 3、在页面中使用

(1).导入插件`import intl from  'react-intl-universal';`

(2).读取语言包`const locales = {"zh-CN": require('./locales/zh-CN.js'),};`

(3).在组件渲染前初始化语言包`intl.init({currentLocale: 'zh-CN',locales,}).then(() => {this.setState({initDone: true});});`

(4).调用`intl.get...`

#### 4、文本获取方式

1. 纯文本获取`intl.get('文本key')`;

2. html文本获取`intl.getHTML('文本key')`;

3. 设置缺省文本`intl.get('not-exist-key').defaultMessage('没有找到这句话')`;

4. 获取带变量的文本，变量文本:`"HELLO": "Hello, {name}. Welcome to {where}!"`，读取方式:`intl.get('HELLO', {name:'banana', where:'China'})`;

5. 文本变量格式化，具体语法为**{变量名, 类型, 格式化}**，下面说几种特殊的变量格式化：
   
   ##### ①.千分位分隔符显示
   
   `"PHOTO": "You have {num ,number,#}"`
   
   ##### ②.数字形式逻辑判断（关键字plural）
   
   `"PHOTO": "You have {num, plural, =0 {no photos.} =1 {one photo.} other {# photos.}}"`
   
   ##### ③.显示货币格式,会自动在值前面加上响应的货币符号
   
   `"SALE_PRICE": "The price is {price, number, USD}"`
   
   ##### ④.日期格式化
   
   `"SALE_START":  "Sale begins {start, date}", "SALE_END":  "Sale ends {end, date, long}"``intl.get('SALE_START',  {start:new  Date()});  // Sale begins 4/19/2017        intl.get('SALE_END',  {end:new  Date()});  // Sale ends April 19, 2017`
   
   **当类型是date的时候**，参数说明：`short`  显示短日期；`medium`  短文本显示月份；`long`  长文本显示月份；`full` 显示最详细的时间。
   
   **当类型是time的时候**，参数说明：`short`  显示时间为小时和分钟；`medium`  显示时间为时分秒；`long`  显示时间为时分秒以及时区。

#### 5、建议

&nbsp;&nbsp;建议创建一个基类组件，其他子组件使用继承的方式，子类重写一个基类方法来实现调用，基类方法如下：

```
import React from 'react'
import intl from 'react-intl-universal';

// 语言配置读取
const locales = {
    "zh-CN": require('./../assets/locales/zh-CN'),
};
/**
 * 功能作用：基类自定义继承的组件，为了做些特殊操作实现的
 * 初始注释时间： 2019/8/29 0029 下午 17:15:30
 * 注释创建人：LorenWang（王亮）
 * 方法介绍：
 * 思路：
 * 修改人：
 * 修改时间：
 * 备注：当前为为了语言初始化使用的
 */
export default class BaseCustomReactComponent extends React.Component {
    state = {initLanguageDone: false}

    /**
     *在渲染初始化语言加载
     */
    componentDidMount() {
        //根据语言读取响应数据
        intl.init({
            // eslint-disable-next-line
            currentLocale: jstlwGetUrlParams("language") === 'en-US' ? 'en-US' : 'zh-CN',
            locales,
        }).then(() => {
            this.setState({initLanguageDone: true});
        });
    }

    /**
     * 子类实现的方法，会在语言初始化之后调用
     */
    renderChild() {
    };

    render() {
        if (this.state.initLanguageDone) {
            return this.renderChild()
        } else {
            return null;
        }
    }
}
```

jstlwGetUrlParams方法为js方法，获取地址：http://file.momentsoflife.work/Common.js
