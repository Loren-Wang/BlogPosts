# HTML5入门（CSS） #

## 1、修改HTML页面的整个背景为充满整个屏幕 ##

在head-->style-->body这样一个路径下

    body{
		background-image: url(img/background.jpg);
		background-size: 100% 100%;
		background-attachment: fixed;
	}



## 2、给输入框添加提示文字（placeholder属性） ##

    <input type="text" placeholder="输入账号" id="inputAccout" />