#ps:本篇文章只讲我自己本人实现的流程，具体原理可能会单开文章讲解，也可能不会写文章讲解，看时间吧#



这里说的是比较简单的一种方法来实现，公共有两种模式使用，一种是本地模式，一种是网络模式，这个和发布到jcenter以及maven中心库的方式不太一样，这个的网络模式使用的是GitHub做载体实现的；


实现：
- 
# 1. 第一步就是新建工程，同时在工程中新建library格式的module项目 #
   这一步其实没必要多说了，只要做安卓开发的人基本上都会做这一步操作，不会的就只能自己去百度了，这个已经很基础了！


# 2.第二步就是开始添加脚本文件  #
   这一步其实执行的自动化，目的是为了生成aar文件来让其他人调用，就和**有些工具生成jar包来给其他人导入调用的效果一样**，aar文件其实就相当于另一种形式的jar包！

   在需要打包的项目中新建一个文件（*ps:记住是项目的根目录，而不是工程的根目录，也就是说library中和builde.gradle同级*），并将文件名以及后缀全称修改为“**maven-release-aar.gradle**”**并在该项目的builde.gradle文件中的结尾中添加一行代码**：`apply from: 'maven-release-aar.gradle'`，这行代码就是要应用脚本文件。（*ps：一定要在结尾当中，否则的话可能无法获取版本号，即versionName*）


# 3.配置maven-release-aar.gradle脚本文件 #
   在文件中添加以下代码（ps:主要在注释）

     // 1.maven-插件
     apply plugin: 'maven'

     // 2.maven-信息
     ext {// ext is a gradle closure allowing the declaration of global properties
         PUBLISH_GROUP_ID = 'com.PicrureOption.android'//框架包名，其实可以是一个总的也可以是开发者域名到写
         PUBLISH_ARTIFACT_ID = 'pictureselect'//框架名称
         PUBLISH_VERSION = android.defaultConfig.versionName//框架版本信息
     }
     
     // 3.maven-输出路径
     uploadArchives {
         repositories.mavenDeployer {
             //这里就是最后输出地址，在自己电脑上新建个文件夹，把文件夹路径粘贴在此
             //注意”file://“ + 路径，有三个斜杠，别漏了
             repository(url: "file:///E:/kuangjia_demo_for_custom/PictureOptionsMaven")

             pom.project {
                 groupId project.PUBLISH_GROUP_ID
                 artifactId project.PUBLISH_ARTIFACT_ID
                 version project.PUBLISH_VERSION
             }
         }
     }
     
     //以下代码会生成jar包源文件，如果是不开源码，请不要输入这段
     //aar包内包含注释
     task androidSourcesJar(type: Jar) {
         classifier = 'sources'
         from android.sourceSets.main.java.sourceFiles
     }
     
     artifacts {
         archives androidSourcesJar
     }


# 4.执行脚本，生成aar文件 #
  在Android studio右侧有个gradle侧边栏，点击会有如下画面，选择要生成aar文件的项目，点击uploadArchives


![](https://raw.githubusercontent.com/Loren-Wang/ArticlePictures/master/%E5%AE%89%E5%8D%93%E5%BC%80%E5%8F%91%E6%89%A9%E5%B1%95%E4%B9%8B%E4%B8%8A%E4%BC%A0library%E5%88%B0maven%E5%9B%BE%E7%89%87_1.png)

  最后生成了aar包，然后去自己配置的路径下查看会查到有和自己versionname相同名称的一个文件夹，下面的就是生成的aar文件包以及jar包源码，如果不想要jar包源码那么根据脚本中的注释进行相应的操作就好！



# 5.本地模式使用 #
  其实执行完成第四步的时候就已经可以通过本地连接进行使用了，使用方法主要在两个文件上**，一个是工程根目录中的builder.gradle**添加以下语句：

     buildscript {
         repositories {
             ...
             maven { url 'file:///E:/kuangjia_demo_for_custom/PictureOptionsMaven'}
             ...
         }
     }
     allprojects {
         repositories {
             ...
             //指定绝对路径
             maven { url 'file:///E:/kuangjia_demo_for_custom/PictureOptionsMaven'}
             ...
         }
     }
     task clean(type: Delete) {
         delete rootProject.buildDir
     }

   第二个是要使用框架的项目中的builder.gradle**添加引入：

     implementation('com.pictureOption.android:pictureselect:1.0.3@aar')
     其中“com.pictureOption.android”是在生成aar文件的时候在脚本中添加的maven信息（PUBLISH_GROUP_ID）；
     其中“pictureselect”是在生成aar文件的时候在脚本中添加的maven信息（PUBLISH_ARTIFACT_ID）；
     其中“1.0.3”是在生成aar文件的时候在脚本中添加的maven信息（PUBLISH_VERSION）；
     其中“@aar”是在使用的时候添加的后缀，当然有些时候是不需要添加的，这个要在使用的时候自己测试下加和不加的情况下是不是能够正常使用；


  基本上如果仅仅是想要在本使用的话到这一步基本上就够了，但是想要使用网络模式的话那还要继续向下看！


# 6.网络模式使用 #

  网络模式使用最重要的一点就是你一定要有一个GitHub账号，没有Github账号你是没有办法上传工程到远端的，就更没有办法说使用网络模式了，如果有了账号，那么就要在远端新建一个工程，将生成的aar文件的根目录（*即E:/kuangjia_demo_for_custom/PictureOptionsMaven*）这个目录下的文件同步上传到GitHub当中，这样才能通过网络模式使用！
  使用网络模式需要在本地模式的基础上修改一个文件，即**工程根目录中的builder.gradle****

     buildscript {
         repositories {
             ...
     //        maven { url 'file:///E:/kuangjia_demo_for_custom/PictureOptionsMaven'}
             maven { url 'https://raw.githubusercontent.com/Loren-Wang/Mavens/master'}
             ...
         }
     }
     allprojects {
         repositories {
             ...
             //指定绝对路径
     //        maven { url 'file:///E:/kuangjia_demo_for_custom/PictureOptionsMaven'}
             maven { url 'https://raw.githubusercontent.com/Loren-Wang/Mavens/master'}
             ...
         }
     }
     task clean(type: Delete) {
         delete rootProject.buildDir
     }

  其实这个url是GitHub上面的仓库地址，仅仅只是把仓库地址中的“github.com”替换为“raw.githubusercontent.com”同时在最后加入“/master”就可以了


# 7.混淆 #
  和正常安卓混淆没什么区别，这里就不多说了


# 8.注意事项 #
  注意路径一定要写对，特别是大小写，路径的大小写不对是找不到文件的，这虽然是低级错误，但其实挺常见的 
  注意Github路径不是常见的https://github.com/flameandroid/mysdk.git，
  而是https://github.com/flameandroid/mysdk/raw/master
  注意如果选择了混淆，切勿在打包那步生成了jar，还上传到Github了，否则白混淆了




参考文章：[https://www.jianshu.com/p/6c1d2688ed2d](https://www.jianshu.com/p/6c1d2688ed2d "https://www.jianshu.com/p/6c1d2688ed2d")
