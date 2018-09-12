       唉！！！可惜了以前写的demo啊，之前写的在来回换电脑之后已经找不到了，看来我还是记下来然后放到github上去吧，要不哪天又找不到了，结果导致不会写了，还好最近稍微空闲了点，可以写点文章和demo，万一项目更新要用到的话有找不到就完了，话不多说，开始吧！
       首先是工具的选择，我使用的是Android Studio开发工具开发，ndk的选择是android-ndk-r13b，至于下载链接以及安装我就不多说了，其实就是解压下来安装就差不多了，如果还不是很明白的话网上有很多的教程(*都到了要学习ndk这一步还弄不好这些基础的话其实就可以重新学习了*)，当然java的开发环境肯定是必须的！
       参考文章：[1](http://blog.csdn.net/shulianghan/article/details/18964835)
               [2](http://www.cnblogs.com/tt2015-sz/p/6148723.html)

步骤：
---

1.新建一个工程，工程名：JNIIntroduction 
--
![这里写图片描述](http://img.blog.csdn.net/20170314140848108?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

2.创建一个java文件用作调用jni方法的入口，并命名为“HelloWorld.java”
--
![创建之后的界面](http://img.blog.csdn.net/20170314141510095?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

3.给工程配置ndk开发包路径
--
![这里写图片描述](http://img.blog.csdn.net/20170314141805299?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

4.添加“android.useDeprecatedNdk=true”
--

在“app/gradle.properties”路径添加的文件中的末尾添加以上的内容(没有这个文件就复制进去一个)，
这一句话必须要加，如果不加这句话会出现下面的错误:

```
Error:Execution failed for task ':app:compileDebugNdk'.
> Error: Your project contains C++ files but it is not using a supported native build system.
Consider using CMake or ndk-build integration with the stable Android Gradle plugin:
 https://developer.android.com/studio/projects/add-native-code.html
or use the experimental plugin:
 http://tools.android.com/tech-docs/new-build-system/gradle-experimental.
```

5.在“app/src/main”文件夹下面创建一个名称为“jni”或“jnilibs”的文件夹用来存放c文件
--
![这里写图片描述](http://img.blog.csdn.net/20170314142720483?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

6.创建“Android.mk”文档
--
如何写 查看文档, NDK根目录下有一个 documentation.html 文档, 点击该html文件就可以查看文档；

本文的文档示例：
![这里写图片描述](http://img.blog.csdn.net/20170314143201178?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

内容含义：

```
-- LOCAL_PATH : 代表mk文件所在的目录;
-- include $(CLEAR_VARS) : 编译工具函数, 通过该函数可以进行一些初始化操作;
-- LOCAL_MODULE : 编译后的 .so 后缀文件叫什么名字;
-- LOCAL_SRC_FILES: 指定编译的源文件名称;
-- include $(BUILD_SHARED_LIBRARY) : 告诉编译器需要生成动态库;
```

7.创建".c"运行文件
--
![这里写图片描述](http://img.blog.csdn.net/20170314144609063?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

内容含义：

```
方法名格式为 : Java_完整包名类名_方法名();
-- JNIEnv参数 : 代表的是Java环境, 通过这个环境可以调用Java里面的方法;
-- jobject参数 : 调用C语言方法的对象, thiz对象表示当前的对象, 即调用JNI方法所在的类;

```

8.配置app的“build.gradle”
--
![这里写图片描述](http://img.blog.csdn.net/20170314144945080?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

9.最后一步就剩调用就好了
--
![这里写图片描述](http://img.blog.csdn.net/20170314145653928?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY213bHk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



源码链接：
-----

https://github.com/Loren-Wang/JNIIntroduction