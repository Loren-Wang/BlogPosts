在开发xmpp中使用的主要是smack+ openfile进行，在使用smack的时候总是报出Smack 报错：**Exception in loadSmackClass**，一开始没顾得上弄这个，后来有点时间了就弄了这个error问题，错误详情：

```
 E/SmackInitialization: Exception in loadSmackClass
 java.io.IOException: No input stream created for 
 classpath:org.jivesoftware.smack.extensions/extensions.providers
 at org.jivesoftware.smack.initializer.UrlInitializer.initialize(UrlInitializer.java:59)
 at org.jivesoftware.smack.SmackInitialization.loadSmackClass(SmackInitialization.java:265)
 at org.jivesoftware.smack.SmackInitialization.parseClassesToLoad(SmackInitialization.java:226)
 at org.jivesoftware.smack.SmackInitialization.processConfigFile(SmackInitialization.java:196)
 at org.jivesoftware.smack.SmackInitialization.processConfigFile(SmackInitialization.java:181)
 at org.jivesoftware.smack.SmackInitialization.<clinit>(SmackInitialization.java:137)
 at org.jivesoftware.smack.SmackConfiguration.getVersion(SmackConfiguration.java:96)
 at org.jivesoftware.smack.ConnectionConfiguration.<clinit>(ConnectionConfiguration.java:38)
```

最后看到了这个**org.jivesoftware.smack.extensions/extensions.providers**错误信息，然后通过全局搜索找到了这个错误所在的父类，最后找到了调用这个类的地方，其中有一个是配置文件**“smack-config.xml”**，然后将报错的注释掉就好了，最后还没有测试会不会出问题，但是登录是正常的，后续还会继续测试的，如果发现问题在上来修改！