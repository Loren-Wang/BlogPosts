本篇文章主要讲的是git操作之合并同一个分支的不同提交信息，即将多个提交记录合并为一个。
这里主要是使用“git rebase”命令，推荐在未提交到远程仓库的时候修改本地记录使用。

步骤：
---
   

一、首 先要切换的要合并commit的分支
二、然后使用命令“git rebase -i HEAD~2”(后面的2代表着要合并的分支数量)，最后出来的界面如下：

```
pick c6e4557 create second.txt
pick e1a7dfa add text in second.txt

# Rebase a71eba2..e1a7dfa onto a71eba2
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out

-----------------------------------ps:---------------------------
ps:第一列是rebase具体执行的操作，其中操作可以选择，其中含义如下:
 - 选择pick操作，git会应用这个补丁，以同样的提交信息（commit message）保存提交
 - 选择reword操作，git会应用这个补丁，但需要重新编辑提交信息
 - 选择edit操作，git会应用这个补丁，但会因为amending而终止
 - 选择squash操作，git会应用这个补丁，但会与之前的提交合并
 - 选择fixup操作，git会应用这个补丁，但会丢掉提交日志
 - 选择exec操作，git会在shell中运行这个命令
```

三、将第二个pick改成squash或者s ，然后保存退出。（在Vim中是按下后“ESC”后输入:wq,最后是按下回车），最后会出现下面的界面：（如果需要修改下提交信息，如果不需要直接保存退出即可。）

```
# This is a combination of 2 commits.
# The first commit's message is:

create second.txt

# This is the 2nd commit message:

add text in second.txt

# 请为您的变更输入提交说明。以 '#' 开始的行将被忽略，而一个空的提交
# 说明将会终止提交。
#
# 日期：  Mon Nov 28 13:59:43 2016 +0800
#
# 变基操作正在进行中；至 a71eba2
# 您在执行将分支 'master' 变基到 'a71eba2' 的操作时编辑提交。
#
# 要提交的变更：
#   新文件：   second.txt
#
```

四、修改成功


ps:部分代码参考自：http://www.cnblogs.com/tocy/p/git-rebase-merge-commit.html

  