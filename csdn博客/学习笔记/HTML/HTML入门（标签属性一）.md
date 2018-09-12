# HTML入门（标签属性一） #


<br>
<br>
## 1、id属性（标签的标记） ##
类似于身份证的作用，标记id所在的标签是哪个，就和java当中的布局中的id属性一样，都是用来标记身份的东西，是唯一的。

语法：`<div  id="版块名称">…</div>`





<br><br><br><br><br><br>
<br>
## 2、summary属性（摘要） ##
现在发现的使用方式是使用在表格当中，是**table**标签的属性，用来标记表格的摘要，可以使表格更容易被搜索引擎读取；至于使用中其他地方有待后续测试。

语法：`<table summary="表格简介文本">`









<br><br><br><br><br>
<br>
## 3、href属性（超链接跳转目标网址属性） ##
超链接跳转目标网址，和**a**标签结合使用，当点击a标签所包含的文字的时候进行页面跳转。

语法：`<a  href="http://www.imooc.com"  title="点击进入慕课网">click here!</a>`


**href属性还可以进行email链接地址的操作，使用mailto进行该操作**，
![](http://img.mukewang.com/52da4f2a000150b714280550.jpg)

**ps:**如果mailto后面同时有多个参数的话，第一个参数必须以“?”开头，后面的参数每一个都以“&”分隔。

















<br>
<br>
<br>
<br>
<br>
<br>
## 4、title属性（鼠标滑过显示的文本） ##
属性所在标签内容被鼠标滑过是的提示文本。

语法：`<a  href="http://www.imooc.com"  title="点击进入慕课网">click here!</a>`













<br><br><br><br><br>
<br>
## 5、target属性（新窗口打开属性） ##
目前暂时只在超链接标签**a**标签当中发现，是点击后新开一个窗口界面加载网址。

语法：`<a href="目标网址" target="_blank">click here!</a>`

<a href="目标网址" target="_blank">click here!</a>










<br><br><br><br><br><br>
## 6、src属性（资源属性） ##
用来引入资源的，在不同标签下这个属性的含义或者使用都不同，例如在**img**标签下是图片资源，在**script**标签下是js文件资源。

语法：`<img src="图片地址" alt="下载失败时的替换文本" title = "提示文本">`

<img src = "https://img3.mukewang.com/user/5333a1200001ff5602000200-40-40.jpg" alt = "My Image" title = "My Image" />







<br><br><br><br><br><br>
## 7、alt属性（描述性文本属性） ##
现在暂时发现是在**img**标签内部，用来指定图像的描述性文本，当图像不可见是，可以看到该属性指定的文本。

语法：`<img src="图片地址" alt="下载失败时的替换文本" title = "提示文本">`

 <img src="http://img.mukewang.com/" alt="sdafsdfsad" title="电影介绍"> 





<br><br><br><br><br><br>
## 8、method属性（表单标签数据传送方式属性） ##
数据的传送方式（get/post）

语法：`<form   method="传送方式"   action="服务器文件">`





<br><br><br><br><br><br>
## 9、action属性（表单标签服务器目标文件或地址） ##
表单内部中浏览着输入的数据会被传动到一个地址，可以使php页面也可以是一个网址等。


语法：`<form   method="传送方式"   action="服务器文件">`






<br><br><br><br><br><br>
## 10、type属性（输入框类型属性） ##
用来设定输入框的输入类型，例如文本输入框（text），密码输入框（password）等，还有一个按钮（button）值，不知道卵用。

语法：` <input type="text/password" name="名称" value="文本" />`


**ps：**

（1）type="text"：文本输入框

（2）type="password":密码输入框

（3）type="radio":单选框

（4）type="checkbox":复选框

（5）type="submit":提交按钮，在表单中只有此类型input才有提交作用

（6）type="reset":重置按钮，在表单中只有此类型的input才有提交作用










<br><br><br><br><br><br>
## 11、name属性（输入框名称） ##
为文本框命名，以备后台程序ASP 、PHP使用。

语法：`<input type="text/password" name="名称" value="文本" />`






<br><br><br><br><br><br>
## 12、value属性（输入框默认值） ##
为文本输入框设置默认值。(一般起到提示作用)

语法：`<input type="text/password" name="名称" value="文本" />`





<br><br><br><br><br><br>
## 13、placeholder属性（输入框、文本域提示） ##
为文本输入框或者文本域设置提示文字，和value文字显示样式不同。

语法：`<input type="text" placeholder="输入账号" id="inputAccout" />`
`<textarea cols="50"placeholder="输入账号"  rows="10"></textarea>`

<input type="text" placeholder="输入账号" id="inputAccout" />
<textarea cols="50"placeholder="输入文本域账号"  rows="2"></textarea>










<br><br><br><br><br><br>
## 14、raws属性（文本域行数定义） ##
用来定义文本域多行输入时的行数。

语法：`<textarea  rows="行数" cols="列数">文本</textarea>
`
<textarea  rows="行数" cols="列数">文本</textarea>








<br><br><br><br><br><br>
## 15、cols属性（文本域列数定义） ##
用来定义文本域多行输入时的列数。

语法：`<textarea  rows="行数" cols="列数">文本</textarea>
`
<textarea  rows="行数" cols="列数">文本</textarea>








<br><br><br><br><br><br>
## 16、checked属性（输入框为单/复选框是的选中状态） ##
使用在**input**标签当中，用来设置单选框或者复选框的选中，当**input**有这个属性的时候该选项会被默认选中，如果有多个单选框都有这个属性的话，最后一个单选框会被默认选中。

语法：`<input   type="radio/checkbox"   value="值"    name="名称"   checked="checked"/>`

<input   type="radio"   value="值"    name="名称"   checked="checked"/>






<br><br><br><br><br><br>
## 17、selected属性（下拉选择列表选中状态） ##
和checked属性一样，下拉选择列表的option标签一旦使用了这个属性，那么该选择则默认被选中。

语法：`<option value="旅游" selected="selected">旅游</option>`







<br><br><br><br><br><br>
## 18、multiple属性（下拉列表多选属性） ##
下拉列表的多选属性，默认情况下回将所有的选项都显示出来进行选择，多选时按下ctrl加单击即可多选。

语法：

    <select multiple="multiple">
      <option value="看书">看书</option>
      <option value="旅游">旅游</option>
      <option value="运动">运动</option>
      <option value="购物">购物</option>
     </select>







<br><br><br><br><br><br>
## 19、for属性（label标签绑定属性） ##
用来绑定其他标签id的，当其值为其他标签id，**lable**使用该属性后会将label的点击时间传递给被绑定的标签使用。

语法：`<label for="控件id名称">`