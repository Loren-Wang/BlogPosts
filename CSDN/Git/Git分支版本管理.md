# Git分支版本管理


&emsp;&emsp;现在主流的代码管理工具基本上就是git了，svn虽然说也有人在用，但是毕竟不是那么的多了，git就不一样了，依旧是在呗大多数人所接受着，国内一般人使用的是**开源中国**的git库管理，也有人在用国外的**GitHub**去做云端的库管理，甚至也可以自己搭建Git管理的中央库，例如**gitlab**等。

&emsp;&emsp;在使用Git去管理最重要的一点就是分支的管理，官网是这么说的：
![enter description here](http://blogpic.momentsoflife.work/1579423612163.png)

&emsp;&emsp;首先我看中的是第二点，基于角色，也就是说git当中每一个分支都是一个角色，各有各的功能，例如开发分支、生产分支、测试分支等等。

&emsp;&emsp;那第一点和第三点是什么意思呢？在我的理解看来第一点和第三点所说的是临时分支，何为临时分支，临时就是用的时候就用不用的时候就删除掉，不会被保留下，这就是临时分支，同样第四点也是差不多的含义，这是**Git**很好用的一个特性，方便开发对某一个单独的功能进行测试。


# 下面来一张我自己所理解到的Git分支管理的流程图


![enter description here](http://blogpic.momentsoflife.work/git分支管理.png)

&emsp;&emsp;下面说下我图里面的所有相关分支信息 

 - ***Master分支：*** **主分支**，用来发布生产环境代码或者说线上代码，只有项目管理员或者发布人员才有权限去进行修改；
 - ***Develop分支：*** **开发分支**，所有的开发人员使用的分支，同样只有项目管理员以及开发人员才能查看，当所有功能都开发完毕之后此分支就是最新的代码分支；
 - ***Test分支：*** **测试分支**，该分支为动态生成，非存储，每次单个版本项目完成之后均会生成或删除该分支以达到代码是最新的目的；
 - ***MasterReady分支：*** **预发布分支**该分支也为动态生成分支，在需要测试之前由脚本生成，同时生成后推送到远端，同时合并**开发/修复以及Master**之后再动态生成测试分支然后进行编译测试；
 - ***Fix分支：*** 修复分支，主要针对于生产环境发生问题需要紧急修复上线时使用，在出现问题之后新建该分支，然后不同开发人员去修复，修复完成之后依旧走开发提交测试的流程；
 - ***devX分支：*** 各个开发人员分支，同样也可以不用直接使用rebase命令在develop分支上操作也可以，在开发完成之后都要合并到develop分支上保持最新代码；
 - ***fixX分支：*** 各个修复bug的开发人员分支，同上；

# 各个开发操作流程

## 开发-测试-发布流程

 1.dev1、dev2、dev3开发人员进行项目功能模块开发以及自测；
 2.开发人员完成开发，**所有代码合并到develop分支，并解决冲突**；
 3.完成开发提交测试申请；
 4.测试通过Jenkins脚本部署代码（部署脚本下面会给出）（**脚本大致流程：**develop-MasterReady--(merge)master--test**）;
 5.测试通过后继续向下走，失败重写修改从第一步从头来；
 6.通过够Master合并MasterReady分支并部署；
 7.部署结束后将master分支反合到开发分支；

## 修复-测试-发布流程

 1.生产环境发现问题修复修复，**直接在master上切换出Fix分支**;
 2.由开发人员修复，修复完成后提交测试申请；
 3.测试通过Jenkins脚本部署代码（部署脚本下面会给出）（**脚本大致流程：**fix-MasterReady--(merge)master--test**）;
 4.测试通过后继续向下走，失败重写修改从第一步从头来；
 5.通过够Master合并MasterReady分支并部署；
 6.部署结束后将master分支反合到开发分支；


# 脚本

## 开发完成后测试部署脚本

``` shell
#!/usr/bin/env bash
#开发人员开发完成后部署测试的脚本

#cd Project 切换到项目根目录

#切换到主分支拉取最新代码
git checkout master
git pull --rebase
#切换到开发分支拉取最新代码
git checkout develop
git pull --rebase
#删除预发布分支
git branch | grep -w "master_ready" | xargs git branch -D
#删除测试分支
git branch | grep -w "test" | xargs git branch -D
#从develop分支上新建预发布分支
git checkout -b master_ready
#合并master分支保证最新代码
git merge master
#当前预发布代码是最新代码，删除远端预发布代码,删除后更新当前代码为远端最新代码
git push origin :master_ready
git push origin master_ready:master_ready
#更新完成后从当前分支上新建测试分支
git checkout -b test
#打印当前最新提交
git log -1


#切换完成之后开始编译代码



```

## 测试结束后部署正式脚本

``` shell
#!/usr/bin/env bash
#测试完成之后的部署脚本

#cd Project 切换到项目根目录

#切换到主分支拉取最新代码
git checkout master
git pull --rebase
#删除当前预发布分支，并从远端检出最新的预发布分支
git pull origin master_ready:master_ready
#合并预发布分支
git merge master_ready
#合并结束后推送最新代码到远端
git push origin origin master:master


#推送完成之后开始部署编译代码


#部署结束之后反合最新代码到develop,先删除本地develop
git branch -D develop
#拉取最新develop代码
git push origin develop:develop
#切换到开发分支合并最新master代码，如果失败也不用处理，成功就推送最新代码到远端
git checkout develop
git merge master
git push origin develop:develop



```

## 修复完成后测试部署脚本

``` shell
#!/usr/bin/env bash
#开发人员开发完成后部署测试的脚本

#cd Project 切换到项目根目录

#切换到主分支拉取最新代码
git checkout master
git pull --rebase
#切换到修复分支拉取最新代码
git checkout fix
git pull --rebase
#删除预发布分支
git branch | grep -w "master_ready" | xargs git branch -D
#删除测试分支
git branch | grep -w "test" | xargs git branch -D
#从develop分支上新建预发布分支
git checkout -b master_ready
#合并master分支保证最新代码
git merge master
#当前预发布代码是最新代码，删除远端预发布代码,删除后更新当前代码为远端最新代码
git push origin :master_ready
git push origin master_ready:master_ready
#更新完成后从当前分支上新建测试分支
git checkout -b test
#打印当前最新提交
git log -1

#切换完成之后开始编译代码

```


# PS：写的不是很好，有问题麻烦指出，谢谢！


