# HTML入门（标签定义二） #

<br>
<br>
## 13、hr标签（水平横线标签） ##
和br换行标签一样，属于特殊标签，可以单独使用，其含义是增加一个水平横线显示。

语法：`<hr>`<hr/>



<br>
<br>
## 14、address标签（地址信息标签） ##
用来对一段文字做标记的，被标记的文字会以特殊形式显示出来，默认为斜体显示，但是也可以用css样式进行修改。这个标签主要用在文本当中存在着地址信息的情况下，例如公司地址，email地址等。

语法：`<address>联系地址信息</address>`
<style>
address{
color:red;
}
</style>
<address>联系地址信息</address>




<br>
<br>
## 15、code标签（单行代码显示标签） ##
被这个标签包含的程序代码会以纯代码的形式显示出来，毕竟有一些网页是需要显示代码的，所以这个时候就要用到这个标签了。

语法：`<code>var i=i+300;</code>`
<code>var i=i+300;</code>

**ps:**这个标签只能用于一行代码插入，当多行代码插入的话就要使用pre标签了；在code的标签当中多个空格的话只会被保留一个空格。







<br>
<br>
## 16、pre标签（多行代码显示标签） ##
这个标签是在遇到多行代码显示的时候使用的，如果没有这个标签的话就只能讲每一行代码都用**code**标签包含起来，那样会很麻烦的。

语法:`<pre>语言代码段</pre>`
<pre>
    var message="欢迎";
    for(var i=1;i<=10;i++)
    {
        alert(message);
    }
</pre>


**ps：**在pre标签当中通常会被保留空格和换行符的。






<br>
<br>
## 17、ul标签（无序列表标签） ##
列表标签，通常用来显示新闻列表等。类似于java当中的listview列表控件；但是这个标签内部的列表是无序的，默认样式一般为自带原点。和li标签组合使用，**li**标签类似于java当中的item。

语法：`<ul>
  <li>信息</li>
  <li>信息</li>
   ......
</ul>`

<ul>
  <li>精彩少年</li>
  <li>美丽突然出现</li>
  <li>触动心灵的旋律</li>
</ul>





<br>
<br>
## 18、ol标签（有序列表标签） ##
这个列表标签会在网页当中展示带有前后顺序的列表信息，默认样式一般是序号显示，从**1**开始排序。


语法：`<ol>
   <li>信息</li>
   <li>信息</li>
   ......
</ol>`

<ol>
   <li>前端开发面试心法 </li>
   <li>零基础学习html</li>
   <li>JavaScript全攻略</li>
</ol>





<br>
<br>
## 19、li标签（列表item标签） ##
这个标签仅仅只用在了ol以及ul标签当中，充当了item的角色，每一组的**li**标签内部都是一行，也代表着一条数据




<br>
<br>
## 20、div标签（逻辑容器标签） ##
在网页制作过程当中，可以把一些独立的逻辑部分划分出来，放在一个**div**标签当中，如果没有div标签所有的逻辑都放在一起的话我估计没几个人能看清，所有这个标签很有必要的，就像有一些网站上模块显示的时候，像新闻一个模块，娱乐一个模块同时显示的时候使用该标签那么久很容易拆分了和开发了。

语法：`<div>…</div>`



<br>
<br>
## 21、table标签（表格标签） ##
有些时候网页要像Excel表格一样展示一些数据，而table标签就是用来在网页上实现这个的，在table标签下有四个子标签，分别是：tbody、th、tr、td。

（1）`<table>…</table>：整个表格以<table>标记开始、</table>标记结束。`

（2）`<tbody>…</tbody>：如果不加<thead><tbody><tfooter> , table表格加载完后才显示。加上这些表格结构， tbody包含行的内容下载完优先显示，不必等待表格结束后在显示，同时如果表格很长，用tbody分段，可以一部分一部分地显示。（通俗理解table 可以按结构一块块的显示，不在等整个表格加载完后显示。）`

（3）`<tr>…</tr>：表格的一行，所以有几对tr 表格就有几行。`

（4）`<td>…</td>：表格的一个单元格，一行中包含几对<td>...</td>，说明一行中就有几列。`

（5）`<th>…</th>：表格的头部的一个单元格，表格表头。`

（6）表格中列的个数，取决于一行中数据单元格的个数。


语法：

    <table>
      <tbody>
        <tr>
          <th>班级</th>
          <th>学生数</th>
          <th>平均成绩</th>
        </tr>
        <tr>
          <td>一班</td>
          <td>30</td>
          <td>89</td>
        </tr>
        <tr>
          <td>二班</td>
          <td>35</td>
          <td>85</td>
        </tr>
        <tr>
          <td>三班</td>
          <td>32</td>
          <td>80</td>
        </tr>
     </tbody>
    </table>

<table>
  <tbody>
    <tr>
      <th>班级</th>
      <th>学生数</th>
      <th>平均成绩</th>
    </tr>
    <tr>
      <td>一班</td>
      <td>30</td>
      <td>89</td>
    </tr>
    <tr>
      <td>二班</td>
      <td>35</td>
      <td>85</td>
    </tr>
    <tr>
      <td>三班</td>
      <td>32</td>
      <td>80</td>
    </tr>
 </tbody>
</table>




<br>
<br>
## 22、caption标签（表格标题标签） ##
一般情况下表格是要有标题和摘要显示的，这个标签的含义是显示表格的标题，默认情况下是显示在表格的上方，同时居中，摘要是不会显示的，同时摘要的添加是使用属性的方式添加的，会在另外一篇文章讲述。

语法：`<caption>标题文本</caption>`

**ps：**使用在table标签内部的子标签形式