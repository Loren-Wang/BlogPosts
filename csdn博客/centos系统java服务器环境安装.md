   首先第一步就是centos系统中需要安装的工具或者环境，包含以下几个选项

   1、java的jdk，这个工具是为了能够让java程序能够正常运行所必须的（不需要tomcat，springboot自己有携带）；

   2、gradle，因为程序使用的gradle环境做的远程项目关联，所以需要使用gradle进行打包编辑；

   3、git版本控制器，用来做远程代码同步使用，所有的代码需要他来保证最新的；

   4、screen远程主机管理器，用来进行远程窗口关闭可以继续运行的工具；

   5、unzip，压缩包解压工具，安装环境使用；

   


# 第一步：系统安装前准备 #
    因为使用的是阿里云服务器，所以就用不上系统本身的防火墙，所以就要将防火墙进行关闭，同时禁止防火墙自启,至于端口开放则在安全组里面设置即可

**关闭防火墙：**

    systemctl stop firewalld.service
    
**关闭防火墙开机自启动功能：**

    systemctl disable firewalld.service







# 第二步：安装jdk #

   **1、下载jdk：**[http://www.oracle.com/technetwork/java/javase/downloads/index.html](http://www.oracle.com/technetwork/java/javase/downloads/index.html "http://www.oracle.com/technetwork/java/javase/downloads/index.html")

    例如：curl -L "http://download.oracle.com/otn-pub/java/jdk/8u172-b11/a58eab1ec242421181065cdc37240b08/jdk-8u172-linux-x64.tar.gz" -H "Cookie: oraclelicense=accept-securebackup-cookie"  -H "Connection: keep-alive" -O


   如果发生了下载了不能用的错误则参考：[https://www.linuxidc.com/Linux/2016-10/135925.htm](https://www.linuxidc.com/Linux/2016-10/135925.htm "https://www.linuxidc.com/Linux/2016-10/135925.htm")



   **2、新建一个目录：**

    mkdir /usr/java

   **3、解压jdk到 /usr/java**

    tar xzf jdk-8u172-linux-x64.tar.gz 

   **4、设置环境变量：**

    （1）编辑 /etc/profile：vi /etc/profile。
    （2）按 i 键进入编辑模式。
    （3）在 /etc/profile 文件中添加以下信息：
             #set java environment
             export JAVA_HOME=/usr/java/jdk1.8.0_172
             export CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib
             export PATH=$JAVA_HOME/bin:$PATH


   **5、加载环境变量**
     
    source /etc/profile

   **6、测试，当出现 jdk 版本信息时，表示 JDK 已经安装成功。**

    java -version








# 第三步：安装gradle #
   **1、下载安装包：**[https://gradle.org/releases/](https://gradle.org/releases/ "https://gradle.org/releases/")

   例如：`wget https://downloads.gradle.org/distributions/gradle-4.8-all.zip`


   **2、新建一个目录：**

    mkdir /usr/gradle

   **3、解压jdk到 /usr/java**

    unzip gradle-4.8-all.zip （如果无法找到unzip那则使用yum命令安装unzip）

   **4、设置环境变量：**

    （1）编辑 /etc/profile：vi /etc/profile。
    （2）按 i 键进入编辑模式。
    （3）在 /etc/profile 文件中添加以下信息：
             #set gradle environment
             GRADLE_HOME=/usr/gradle/gradle-4.8
             export PATH=${GRADLE_HOME}/bin:${PATH}


   **5、加载环境变量**
     
    source /etc/profile

   **6、测试，当出现 gradle 版本信息时，表示 gradle 已经安装成功。**

    gradle -version



# 第四步：安装git #

   这个安装很简单：`yum install git`一行代码就ok


# 第五步：安装screen #
  和第四步一样：`yum install screen`