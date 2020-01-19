# Intellij 在编译的时候报错“Cause: zip END header not found”**



实际的错误如下图：

![https://raw.githubusercontent.com/Loren-Wang/PictureList/master/csdn%E5%8D%9A%E5%AE%A2/idea_zip_error.png](https://raw.githubusercontent.com/Loren-Wang/PictureList/master/csdn%E5%8D%9A%E5%AE%A2/idea_zip_error.png)





其实原因就是你所使用的gradle的版本的zip压缩包被损坏，而不是什么java的zip损坏，你只要修改你的gradle版本就好，或者直接指定你本地的某一个gradle都可以，但是不要是相同版本的！



修改路径：${project}/gradle/wrapper/gradle-wrapper.properties 内的distributionUrl版本或使用本地路径
