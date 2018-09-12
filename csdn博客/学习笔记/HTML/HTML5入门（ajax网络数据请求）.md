# HTML5入门（ajax网络数据请求） #

**参考文章地址：**[http://www.w3school.com.cn/jquery/ajax_ajax.asp](http://www.w3school.com.cn/jquery/ajax_ajax.asp)

**jquery官网地址：**[https://jquery.com/download/](https://jquery.com/download/)


## 使用步骤 ##

  1、在要发起网络请求的HTML文件当中引入jquery工具

    <script src="https://code.jquery.com/jquery-3.3.1.js"></script>
    或
    <script src="netrequest/jquery-3.3.1.js"></script>

  
  2、在引入jquery的下方的任意一个script标签当中创建网络请求的fanction方法
 
    <script>
		function testNetRequest(){}
	</script>

  3、构建请求方法（ajax内部的属性定义以及使用请参考上面的参考文章地址）
     
    <script>
		function testNetRequest(){
			$.ajax({
				type:"get",
				url:"https://www.baidu.com",
				async:true,
				success:function(data){
					alert(data)
				},error:function(data){
					alert(data)
				}
			});
		}
	</script>

  4、使用
    
    直接通过onclick方法等方式调用该function即可


   
  

ps：如果想要将ajax的网络请求统一到一个js文件中则参考本人的另外一篇文章------HTML5入门（js代码合并）