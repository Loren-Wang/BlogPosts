Android的消息传递机制

1、安卓的消息传递有哪些

2、Content Provider

3、广播（Broadcast）

4、AIDL Service

5、Websocket

6、XMPP

# 一、安卓的消息传递机制有哪些

**1.线程间通讯 ——— Handler，HandlerThread等。**  
**2.组件间通信 ——— BroadcastReceiver，接口回调等。**  
**3. 第三方通信 ——— EventBus，rxBus**  
**4.进程间通信 ——— Content Provider ，Broadcast ，AIDL等。**  
**5.长连接推送 ——— WebSocket，XMPP等。**

# 二、Content Provider

&emsp;&emsp;Android应用程序可以使用文件或SqlLite数据库来存储数据。Content Provider提供了一种在多个应用程序之间数据共享的方式（跨进程共享数据）。应用程序可以利用Content Provider完成下面的工作  

1. 查询数据  
2. 修改数据  
3. 添加数据  
4. 删除数据  

&emsp;&emsp;虽然Content Provider也可以在同一个应用程序中被访问，但这么做并没有什么意义。Content Provider存在的目的向其他应用程序共享数据和允许其他应用程序对数据进行增、删、改操作。  

&emsp;&emsp; Android系统本身提供了很多Content Provider，例如，音频、视频、联系人信息等等。我们可以通过这些Content Provider获得相关信息的列表。这些列表数据将以Cursor对象返回。因此，从Content Provider返回的数据是二维表的形式。

# 三、广播（Broadcast）

&emsp;&emsp;广播是一种被动跨进程通讯的方式。当某个程序向系统发送广播时，其他的应用程序只能被动地接收广播数据。这就象电台进行广播一样，听众只能被动地收听，而不能主动与电台进行沟通。  
&emsp;&emsp;在应用程序中发送广播比较简单。只需要调用sendBroadcast方法即可。该方法需要一个Intent对象。通过Intent对象可以发送需要广播的数据。  
&emsp;&emsp;注意普通的Service并不能实现跨进程操作，实际上普通的Service和它所在的应用处于同一个进程中，而且它也不会专门开一条新的线程，因此如果在普通的Service中实现在耗时的任务，需要新开线程。  
&emsp;&emsp;要实现跨进程通信，需要借助AIDL(Android Interface Definition Language)。Android中的跨进程服务其实是采用C/S的架构，因而AIDL的目的就是实现通信接口。

# 四、AIDL Service

&emsp;&emsp;这是我个人比较推崇的方式，因为它相比Broadcast而言，虽然实现上稍微麻烦了一点，但是它的优势就是不会像广播那样在手机中的广播较多时会有明显的时延，甚至有广播发送不成功的情况出现。  

# 五、Websocket

&emsp;&emsp;支持客户端和服务器端的双向通信，而且协议的头部又没有HTTP的Header那么大，于是，Websocket就诞生了！

&emsp;&emsp;Websocket是应用层第七层上的一个应用层协议，它必须依赖 HTTP 协议进行一次握手 ，握手成功后，数据就直接从 TCP 通道传输，与 HTTP 无关了。

&emsp;&emsp; Websocket的数据传输是frame形式传输的，比如会将一条消息分为几个frame，按照先后顺序传输出去。这样做会有几个好处：

&emsp;&emsp;1 大数据的传输可以分片传输，不用考虑到数据大小导致的长度标志位不足够的情况。

&emsp;&emsp; 2 和http的chunk一样，可以边生成数据边传递消息，即提高传输效率。

# 六、XMPP

&emsp;&emsp; XMPP中定义了三个角色，客户端，服务器，网关。通信能够在这三者的任意两个之间双向发生。服务器同时承担了客户端信息记录，连接管理和信息的路由功能。网关承担着与异构即时通信系统的互联互通，异构系统可以包括SMS（短信），MSN，ICQ等。基本的网络形式是单客户端通过TCP/IP连接到单服务器，然后在之上传输XML。
