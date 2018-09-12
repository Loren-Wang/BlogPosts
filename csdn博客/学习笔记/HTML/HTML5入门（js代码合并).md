# HTML5入门（js代码合并) #

   有些时候在开发过程当中会遇到很多的重复的js代码，就拿网络请求而言，一个项目当中的网络请求数量一般情况下不会小于10个，也就是说正常情况下你要写10遍网络请求才可以，那么问题就来了，10个20个请求好说，但是50个100个呢？需要写多少的重复代码，一旦请求需要统一修改的时候你要修改多少东西？这就造成了维护上的麻烦，所以类似的将过于多的重复代码整合到一起，通过一个文件进行调用的话会怎样？那会节省很多代码填写，代码的整洁度以及可维护性会相应的增高！



## 1、抽取后保存的文件注释 ##
   每个人呢都有每个人的习惯，这个基本上没法改变的，当然的代码的基础还是不变的，仅仅只是使用上以及标注上的差别，所以这里所有的注释都是我个人的使用习惯，当然了，也是会参考别人的习惯，取长补短，在开发这一途闭门造车可不是什么好习惯！

**在文件的第一行开始要注释以下内容：**（当然后续可能会有变动，变动的haul不一定会发布新的）
    
    /**
     * 创建人：
     * 创建时间：
     * 描述：
     * 参考：
     * 功能：
     */

   **ps:对于一个程序员来说，不单单代码写的漂亮就行，还要注意注释的问题，否则的话代码写的再漂亮有什么用，别人也看不明白，毕竟看你代码的不单单只有和你一起开发的人**
    
    
    
    
    
## 2、将被抽取的通用代码放到一个函数当中 ##
  
   此时需要将可以被通用的代码放到一个函数当中，当然了，通用代码可能不止一部分代码，可能是由多个代码块组成，那么就将多个代码块放到多个函数当中使用，所以这个时候就要定义多个函数了！（**当然了，post请求未测试，只是仿照get请求随便写了下，只是为了概念**）
    

    //请求超时时间
    var TIME_OUT = 100;//单位毫秒
    
    /**
     * 发起get网络请求
     * @param {String} url 请求网址
     * @param {Function} completeFunction 请求完成后回调函数 (请求成功或失败之后均调用)。
     * @param {Function} successFunction 成功回调
     * @param {Function} failFunction 失败回调
     */
    function getRequest(url,completeFunction, successFunction,failFunction) {
    	$.ajax({
    		type: "get",
    		url: url,
    		async: true,
    		timeout: TIME_OUT,
    		complete: function(data) {
    			successFunction(data)
    		},
    		success: function(data) {
    			successFunction(data)
    		},
    		error: function(data) {
    			successFunction(data)
    		}
    	});
    }

    /**
     * 发起post网络请求
     * @param {String} url 请求网址
     * @param {Function} completeFunction 请求完成后回调函数 (请求成功或失败之后均调用)。
     * @param {Function} successFunction 成功回调
     * @param {Function} failFunction 失败回调
     */
    function postRequest(url,completeFunction, successFunction,failFunction) {
    	$.ajax({
    		type: "post",
    		url: url,
    		async: true,
    		timeout: TIME_OUT,
	    	complete: function(data) {
	    		successFunction(data)
	    	},
	    	success: function(data) {
	    		successFunction(data)
	    	},
	    	error: function(data) {
	    		successFunction(data)
	    	}
	    });
    }



## 3、引入js文件 （也包括两个js文件的互相调用）##

  使用js文件很方便的，直接在html文件当中使用script标签引入即可，但是要注意，如果用到了其他js文件的东西的话那么就要在同一个HTML当中引入另外一个js文件，如下：
    
    //ajax使用的引入
    <script src="netrequest/jquery-3.3.1.js"></script>
    //抽取出来的代码所在的js文件
	<script src="netrequest/netRequest.js"></script>


## 4、被抽取合并的代码的使用 ##

 既然已经将相同代码抽取并合并到了一个文件，同时也将文件引入到了HTML当中，那么剩下的就按照function方法的形式使用就行了