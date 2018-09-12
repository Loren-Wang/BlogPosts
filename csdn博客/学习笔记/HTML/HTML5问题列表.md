# HTML5问题列表 #


## 1."Uncaught ReferenceError: $ is not defined" ##
   
**问题原因：**缺少***jquery***文件的导入，该文件的导入需要在每一个使用到该文件内容的HTML当中导入

**解决办法：**在HTML当中使用script标签引入jsqury即可，引入方式分为两种，一种是将jsqury.js文件复制到项目当中并使用`<script src="jquery-3.3.1.js"></script>`方式引入，一种是直接网络引入`<script src="https://code.jquery.com/jquery-3.3.1.js"></script>`，但是有一点要注意，**引用一定要在使用之前，否则还是会报这个错误**

  

**jquery官方网站地址:**[https://jquery.com/download/](https://jquery.com/download/ "https://jquery.com/download/")

----------
