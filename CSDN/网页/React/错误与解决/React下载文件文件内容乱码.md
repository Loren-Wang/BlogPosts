使用axios进行文件下载

&nbsp;&nbsp;文件下载后内容乱码的主要原因是没有设置responseType，因为构造blob不知道何种原因总是对于构造后的数据是乱码的。

&nbsp;&nbsp;因为存储从服务去返回的二进制文件流就必须要永达blob，但是接口自主构造却不行，所以这里就只能让响应实体返回blob，在请求的config中设置responseType：‘blob’，这样的会就会在response中返回blob，但是这个blob是data，而不是像feach中是个单独的方法。





![](https://raw.githubusercontent.com/Loren-Wang/BlogPosts/master/csdn%E5%8D%9A%E5%AE%A2/ArticlePicture/react%E6%96%87%E4%BB%B6%E4%B8%8B%E8%BD%BD.png)




