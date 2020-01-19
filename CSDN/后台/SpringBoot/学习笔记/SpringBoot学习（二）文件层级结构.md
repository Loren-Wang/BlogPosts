![](https://raw.githubusercontent.com/Loren-Wang/ArticlePictures/master/SpringBoot%E5%AD%A6%E4%B9%A0%EF%BC%88%E4%BA%8C%EF%BC%89%E6%96%87%E4%BB%B6%E5%B1%82%E7%BA%A7%E7%BB%93%E6%9E%84_1.png)

上图所示的就是**springboot**开发中整个的文件层级结构了，

1、最上层的**build.gradle**文件是gradle的构建文件，里面有一些项目用到的一些属性，例如说用到了那些第三方框架，版本信息等，如果是maven项目的话那么文件名就会叫做pom.xml了；

2、**src：**这层里面其实就是所有的代码以及资源了，是整个项目的主体所在；

3、**main：**注册是正式代码所在的层；

4、**java：**我理解到的是下面的代码是java代码，如果是kotlin代码的话，这个名字可能就会变成kotlin了呢，其实就像是区分作用，不过在java下也可能会出现kotlin代码的；

5、**Myapp：**这一层的含义就有点像是安卓开发当中的包名了，这个也许是一层也有可能是多层；

6、**Application.java**一个带有main（）方法的类，用于引导启动程序，和所有的java开发一样；

7、**resources：**所有的资源文件所在的目录；

8、**application.properties**一个空的properties文件，用来根据需要添加配置信息；

9、**static：**放置的是web应用程序的静态内容（JavaScript、样式表、图片等）；

10、**templates：**呈现模型数据的模板；

11、**ApplicationTests.java**一个空的JUnit测试类，他加载了一个使用springboot自动配置功能的spring应用程序上下文