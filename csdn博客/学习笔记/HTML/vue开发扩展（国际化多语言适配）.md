# vue开发扩展（国际化多语言适配） #

参考网址：[https://blog.csdn.net/joyce_lcy/article/details/78840371](https://blog.csdn.net/joyce_lcy/article/details/78840371)


首先一个项目除非初始定义就是在国内，否则的话任何文字相关的信息是都不能写在硬代码当中的，当然了注释除外。

**为什么文字相关的东西不能写在硬代码当中呢？**你要这样来想，如果所有的文字相关都写在了硬代码当中，那么一旦要做国际化适配的时候你是要再写一遍项目呢还是再写一遍呢？再写一篇是一项很消耗人力物力的事情，所以，不提倡将文本写在硬代码当中，那么有人就问了，如果不再写一遍项目那么有什么办法能够解决这个多语言的问题呢？往下看！！！！！！！！！！！！！


<br><br><br><br>
## 1、多语言适配方式 ##
  现阶段不管是web项目还是安卓项目还是ios项目，做国际化的唯一方式就是使用语言包，也就是将同一个语言的文本信息放到同一个文件当中，当切换语言环境的情况下读取不同的语言文件即可！这样就会很方便了，不用重写项目，只要写一份语言包即可。


<br><br><br><br>
## 2、vue实现多语言的工具 ##
vue实现多语言的方式使用的是一个叫做**i18n**的插件。
Vue I18n是Vue.js的国际化插件。它可以轻松地将一些本地化功能集成到您的Vue.js应用程序中。**功能包括：**
各种格式本地化、
多元化、
DateTime本地化、
号码本地化、
基于组件的本地化、
分量插值、
后备本地化、
......等

描述文档：[http://kazupon.github.io/vue-i18n/introduction.html](http://kazupon.github.io/vue-i18n/introduction.html)





<br><br><br><br>
## 3、vue-i18n兼容性 ##
支持Vue.js 2.x以上版本




<br><br><br><br>
## 4、vue-i18n使用步骤（一、插件导入） ##
首先要切换到项目内的根目录当中，执行`npm install vue-i18n`命令，安装插件，但是不知道为什么我的命令不管用，所以使用了另外一种方式：**在index.html文件当中使用script引入i18n**
   <!--引入国际化vue插件-->
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-i18n/dist/vue-i18n.js"></script>




<br><br><br><br>
## 5、vue-i18n使用步骤（二 、制作语言文件） ##
在项目的src目录下根据项目情况自己定义一个文件夹用来存放语言文件，这个文件没啥大要求，最大的要求就是内部的数据格式是要json字符串格式：如下：

    {
     "message":{
        "hello":"你好"
      }
    }

**ps:**最外层的参数一定是要message，然后内部才是要使用的字符串。




<br><br><br><br>
## 6、vue-i18n使用步骤（三 、声明语言文件） ##
不管做什么事情你都要打个招呼，声明一下，要不人家怎么知道有你这么个事情呢，而且语言文件有事全局使用的，所以声明就要放在**main.js**文件当中了。

**声明的第一步**就是要在当前文件当中引入i18n了，上面的插件导入只是到项目当中，那个是无法直接使用的：`import VueI18n from 'vue-i18n'`；

**声明的第二步**就是要创建变量供项目使用了，毕竟没有一个实际的东西谁知道你在哪里呢
    
    //初始化国际化插件
    Vue.use(VueI18n);
    //初始化插件变量
    const i18n = new VueI18n({
      locale:"en",//当前语言环境
      messages:{//语言文件对应语言环境的map集合，value值代表着语言文件的相对地址
    zh:require('./value/language/zh.json'),
    en:require('./value/language/en.json')
      }
    })

**ps：**语言环境的相对地址前面一定要加    <em>"./"</em>这个字符，否则无法找到路径。




<br><br><br><br>
## 7、vue-i18n使用步骤（四 、传递语言变量） ##
既然已经声明完了那就使用就好了，但是想要使用得先把变量传递给主调用方法当中，只要将声明的变量放入到**main.js**文件当中的`new vue（{}）`当中即可；
例如：

    new Vue({
      el: '#app',
      i18n, 
      router,
      components: { App },
      template: '<App/>'
    })




<br><br><br><br>
## 8、vue-i18n使用步骤（五 、在HTML当中使用） ##
使用方式简单，在标签中使用方式：`h3 {{ $t("message.hello") }}    <span v-text="$t('message.hello')"></span>`；在js当中使用：`this.$t("message.welcome")`





<br><br><br><br>
## 9、vue-i18n使用步骤（六 、动态切换语言） ##
实际项目中，往往需要动态切换语言，比如当用户点击了其需要的语言。

vue-i18n 提供了一个全局配置参数叫 “locale”，通过改变 locale 的值可以实现不同语言的切换。

在页面中只需要在切换时，修改this.$i18n.locale的值即可。

例如：`this.$i18n.locale='zhCHS'`