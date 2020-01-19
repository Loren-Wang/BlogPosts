# vue开发扩展（自定义通用js文件并引入） #

参考文件：[https://blog.csdn.net/liuxing393724034/article/details/80775065](https://blog.csdn.net/liuxing393724034/article/details/80775065)


<br><br><br><br>
## 1、定义js文件 ##
既然要使用通用js文件那么肯定是要定义一个js文件作为通用方法的载体，但是这里面的定义和HTML当中有些区别的，格式定义如下：
    
    export defaut{
         //被调用的方法
         test(msg){
            console.log(msg);
         },
    }




**ps：**这个js文件内部可以使用多个export，但是需要在default后面增加多个命名。




<br><br><br><br>
## 2、js文件引入 ##
这个时候的引入分两种引入，一种是主入口引入，在**main.js**当中引入；另外一种是在布局当中引入。

不过两种引入的import方式相同，如下：

    import Comjs from './js/common.js'     //引入公用js

    //如果js当中定义了多个export的话需要指定导入的是哪个import
     import {title} from './js/common.js'   //可以选择需要的方法引入




<br><br><br><br>
## 3、已引入的js文件使用 ##