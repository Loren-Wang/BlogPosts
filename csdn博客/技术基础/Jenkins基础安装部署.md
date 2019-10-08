&nbsp;&nbsp;本文介绍的是在Linux端安装Jenkins工具，该工具是一个开源的自动化的服务器，他可以定时的执行某些任务，也可以定时执行某些脚本任务，例如：当一个项目开发完毕之后，需要在服务器当中更新最新版本代码，那么用Jenkins执行一个特定的脚本就可以了，同时他也可以对失败情况等做处理，总而言之，他其实是一个很强大的工具。

jenkins官网下载：[https://jenkins.io/zh/download/](https://jenkins.io/zh/download/)

Jenkins下载地址：[https://pkg.jenkins.io/redhat-stable/](https://pkg.jenkins.io/redhat-stable/)

#### 1、下载工具包

使用wget命令在上面的下载地址当中下载响应的安装包

#### 2、安装jdk，如果已经安装了可以跳过

&nbsp;&nbsp;因为Jenkins是运行在jdk上面的，所以安装Jenkins之前必须安装jdk。

**安装过程可以搜索或者查看我的另外文章，另外文章写完之后会更新到这里。**

#### 3、安装jenkins

&nbsp;&nbsp;进入到jenkins文件的下载目录，使用`sudo rpm -ivh 文件名称`安装jenkins，之前我试过安装到指定目录，但是没有成功，所以就直接使用默认安装了。

#### 4、jenkins安装目录

1. /usr/lib/jenkins/：jenkins安装目录，WAR包会放在这里。

2. /etc/sysconfig/jenkins：jenkins配置文件，“端口”，“JENKINS_HOME”等都可以在这里配置。

3. /var/lib/jenkins/：默认的JENKINS_HOME。

4. /var/log/jenkins/jenkins.log：Jenkins日志文件。

5. /etc/init.d/jenkins：jenkins的配置文件，java环境变量配置等

#### 5、安装之后检查

&nbsp;&nbsp;安装之后最好检查下jdk环境变量是否配置以及jenkins配置的jdk路径是否正确，不正确的话可能导致jenkins无法启动，**检查文件为：/etc/init.d/jenkins**，通过**vi**命令打开，至于怎么修改这个应该都会，不会其实也没法搞linux，因为这很基础。（可以参考：[https://blog.csdn.net/xishaoguo/article/details/88577459](https://blog.csdn.net/xishaoguo/article/details/88577459)）

#### 6、启动jenkins

`sudo service jenkins start`

&nbsp;&nbsp;启动后要注意如果是用的云服务器的话要将相应的jenkins端口开放，否则在外部是无法访问的，或者如果用的nginx的话可以用代理转发的方式去访问jenkins，这样的话就可以不用开放其他端口，同样，用nginx也可以做域名访问然后通过代理转发的。

&nbsp;&nbsp;**ps：通过nginx代理转发最终要的一点就是在域名或者端口号的后面不要加任何的额外路径，否则的话会导致部分的css或者js资源文件无法访问，以至于显示异常。**

#### 7、启动后问题

&nbsp;&nbsp;启动后除了上面的会碰到的问题之外还有一些很坑的问题，其中之一就是初始化，在访问**Http://服务器ip:jenkins端口号**的时候会出现解锁功能，这个时候就要找密码了，密码所在位置`cat /root/.jenkins/secrets/initialAdminPassword`打印密码，然后输入到网页中解锁。

&nbsp;&nbsp;后面的初始化使用默认就好了，他会执行很长时间，这个要做好准备，至于用户使用自己新建或者只用默认的admin都可以，这个无所谓的。

&nbsp;&nbsp;在初始化后会出现登录页面，下个问题就会出现在登录页面了，那就是登录之后是空白页面，这个时候就难办了，处理流程如下：

1. 输入网址：`Http://服务器ip:jenkins端口/jenkins/pluginManager/advanced`

2. 滑动到页面底部；

3. 最底下有个【升级站点】，把其中的链接改成**Http://updates.jenkins.io/update-center.json**;

4. 然后在服务列表中关闭jenkins，再启动即可
   
   **ps：参考**[https://www.cnblogs.com/me1105/p/10081291.html](https://www.cnblogs.com/me1105/p/10081291.html)
