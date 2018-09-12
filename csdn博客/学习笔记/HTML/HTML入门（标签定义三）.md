# HTML入门（标签定义三） #

<br>
<br>
## 23、a标签（链接标签） ##
使用`<a>`标签可实现超链接，它在网页制作中可以说是无处不在，只要有链接的地方，就会有这个标签。

语法：`<a  href="目标网址"  title="鼠标滑过显示的文本">链接显示的文本</a>`

<a  href="http://www.imooc.com"  title="点击进入慕课网">click here!</a>

**ps:**默认情况下a标签点击后是在本页面新加载一个连接。如果要在新窗口打开链接那么需要属性**target="_blank"**








<br><br><br><br><br><br>
## 24、img标签（图片标签） ##
在网页中插入图片既是美化网址的需要也是必要的条件，试想下如果通篇文本的情况除了小说估计没什么人可以看得下去了。

img标签必须要有一个属性，那就是src属性，这个属性是为为这个标签添加图片信息的，不管是本地的还是在线的。

语法：`<img src="图片地址" alt="下载失败时的替换文本" title = "提示文本">`

<img src = "https://img3.mukewang.com/user/5333a1200001ff5602000200-40-40.jpg" alt = "My Image" title = "My Image" />







<br><br><br><br><br><br>
## 25、form标签（表单标签） ##
网站怎样与用户进行交互？答案是使用HTML表单(form)。表单是可以把浏览者输入的数据传送到服务器端，这样服务器端程序就可以处理表单传过来的数据。

语法：`<form   method="传送方式"   action="服务器文件">`

**ps：**

（1）属性描述见另外的属性文章

（2）所有的表单控件(文本框、文本域、按钮等)都必须房子**form**标签中间，否则输入信息可能无法上传






<br><br><br><br><br><br>
## 26、input标签（输入框标签） ##
和java当中的Edittext类似，可以指定不同的输入类型，同时也可以变成button（虽然暂时还不知道卵用），具体属性见属性文章博客

语法：` <input type="text/password" name="名称" value="文本" />`
<input type="text/password" name="名称" value="文本" />









<br><br><br><br><br><br>
## 27、textarea标签（文本域标签） ##
用户需要输入大段文字的时候就需要用到了文本域了，它可以进行多行文本输入，就像意见反馈一样，需要输入很多文字，他的特殊属性，那就是raws、cols，分别代表着最大输入的行数和列数，具体在属性表中查看。

语法：`<textarea  rows="行数" cols="列数">文本</textarea>`

<textarea  rows="行数" cols="列数">文本</textarea>






<br><br><br><br><br><br>
## 28、select标签（下拉选择列表） ##
下拉列表选择，可以有效节省空间，内部会有子标签option，每一个子标签代表着一个选项。默认情况下为单选。


语法：

     <select>
      <option value="看书">看书</option>
      <option value="旅游">旅游</option>
      <option value="运动">运动</option>
      <option value="购物">购物</option>
    </select>


 <select>
      <option value="看书">看书</option>
      <option value="旅游">旅游</option>
      <option value="运动">运动</option>
      <option value="购物">购物</option>
 </select>






<br><br><br><br><br><br>
## 29、option标签（下拉选择列表item） ##
用来显示下拉选择列表的选项，每一对该标签所包括的内容都是一个选项。










<br><br><br><br><br><br>
## 30、label标签（可用性标签） ##
这个标签在我看来做大的作用就是可以变成任意控件的点击事件，它有一个**for**属性，这个属性用来和控件id做绑定，达到点击label标签执行绑定的标签的点击操作。

语法：`<label for="male">男</label>
  <input type="radio" name="gender" id="male" />`

