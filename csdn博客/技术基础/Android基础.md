# Android基础

1、Activity生命周期(参考：[https://blog.csdn.net/xiajun2356033/article/details/78741121](https://blog.csdn.net/xiajun2356033/article/details/78741121 "https://blog.csdn.net/xiajun2356033/article/details/78741121"))
-------------

  **activity的四个状态：**    
  **(1).** running->当前显示在屏幕的activity(位于任务栈的顶部)，用户可见状态。  
  **(2).** poused->依旧在用户可见状态，但是界面焦点已经失去，此Activity无法与用户进行交互。(onPause（）)  
  **(3).** stopped->用户看不到当前界面,也无法与用户进行交互 完全被覆盖.  
  **(4).** killed->当前界面被销毁，等待这系统被回收  
  **生命周期执行顺序：**  
  **(1).** Starting ——–>Running 所执行的生命周期顺序 onCreate()->onstart()->onResume()  
  (当前称为活动状态（Running），此activity所处于任务栈的top中，可以与用户进行交互。)  
  **(2).** Running ——>Paused 所执行Activity生命周期中的onPause（） 
  (当前称为暂停状态（Paused），该Activity已失去了焦点但仍然是可见的状态(包括部分可见)。)  
  **(3).** Paused ——>Running所执行的生命周期为:OnResume()  
  (当前重新回到活动状态(Running),此情况用户操作home键，然后重新回到当前activity界面发生。)  
  **(4).** Paused ——>Stoped所执行的生命周期为:onStop()  
  (该Activity被另一个Activity完全覆盖的状态,该Activity变得不可见，所以系统经常会由于内存不足而将该Activity强行结束。)  
  **(5).** Stoped——>killed所执行的生命周期为:onDestroy()  
  (该Activity被系统销毁。当一个Activity处于暂停状态或停止状态时就随处可能进入死亡状态，因为系统可能因内存不足而强行结束该Activity。)  

   ![](http://hi.csdn.net/attachment/201109/1/0_1314838777He6C.gif)

  **onCreate():**   
  当我们点击activity的时候，系统会调用activity的oncreate()方法，在这个方法中我们会初始化当前布局setContentLayout（）方法。   
  **onStart():**   
  onCreate()方法完成后，此时activity进入onStart()方法,当前activity是用户可见状态，但没有焦点，与用户不能交互，一般可在当前方法做一些动画的初始化操作。   
  **onResume():**   
  onStart()方法完成之后，此时activity进入onResume()方法中，当前activity状态属于运行状态 (Running)，可与用户进行交互。   
  **onPouse()**   
  当另外一个activity覆盖当前的acitivty时，此时当前activity会进入到onPouse()方法中，当前activity是可见的，但不能与用户交互状态。   
  **onStop()**   
  onPouse()方法完成之后，此时activity进入onStop()方法，此时activity对用户是不可见的，在系统内存紧张的情况下，有可能会被系统进行回收。所以一般在当前方法可做资源回收。   
  **onDestory()** 
  onStop()方法完成之后，此时activity进入到onDestory()方法中，结束当前activity。   
  **onRestart()** 
  onRestart()方法在用户按下home()之后，再次进入到当前activity的时候调用。调用顺序onPouse()->onStop()->onRestart()->onStart()->onResume().  

  **当AActivity切换BActivity的所执行的方法：**  
  AActivity:onCreate()->onStart()->onResume()->onPouse()   
  BActivity:onCreate()->onStart()->onResume()   
  AActivity:onStop()->onDestory()  
  **当AActivity切换BActivity（此activity是以dialog形式存在的）所执行的方法:**  
  AActivity:onCreate()->onStart()->onResume()->onPouse()   
  BActivity:onCreate()->onStart()->onResume()  

  **onSaveInstanceState(Bundle outState):**  
  调用时机 :   
  Activity 被销毁的时候调用, 也可能没有销毁就调用了;   
  按下Home键 : Activity 进入了后台, 此时会调用该方法;   
  按下电源键 : 屏幕关闭, Activity 进入后台;   
  启动其它 Activity : Activity 被压入了任务栈的栈底;   
  横竖屏切换 : 会销毁当前 Activity 并重新创建；  

2、fragment生命周期
-------------

![](https://images2015.cnblogs.com/blog/945877/201611/945877-20161123093212096-2032834078.png)

  **setUserVisibleHint()：** 设置Fragment可见或者不可见时会调用此方法。在该方法里面可以通过调用getUserVisibleHint()获得Fragment的状态是可见还是不可见的，如果可见则进行懒加载操作。  
  **onAttach()：** 执行该方法时，Fragment与Activity已经完成绑定，该方法有一个Activity类型的参数，代表绑定的Activity，这时候你可以执行诸如mActivity = activity的操作。  
  **onCreate()：** 初始化Fragment。可通过参数savedInstanceState获取之前保存的值。  
  **onCreateView()：** 初始化Fragment的布局。加载布局和findViewById的操作通常在此函数内完成，但是不建议执行耗时的操作，比如读取数据库数据列表。  
  **onActivityCreated()：** 执行该方法时，与Fragment绑定的Activity的onCreate方法已经执行完成并返回，在该方法内可以进行与Activity交互的UI操作，所以在该方法之前Activity的onCreate方法并未执行完成，如果提前进行交互操作，会引发空指针异常。  
  **onStart()：** 执行该方法时，Fragment由不可见变为可见状态。  
  **onResume()：** 执行该方法时，Fragment处于活动状态，用户可与之交互。  
  **onPause()：** 执行该方法时，Fragment处于暂停状态，但依然可见，用户不能与之交互。  
  **onSaveInstanceState()：** 保存当前Fragment的状态。该方法会自动保存Fragment的状态，比如EditText键入的文本，即使Fragment被回收又重新创建，一样能恢复EditText之前键入的文本。  
  **onStop()：** 执行该方法时，Fragment完全不可见。  
  **onDestroyView()：** 销毁与Fragment有关的视图，但未与Activity解除绑定，依然可以通过onCreateView方法重新创建视图。通常在ViewPager+Fragment的方式下会调用此方法。  
  **onDestroy()：** 销毁Fragment。通常按Back键退出或者Fragment被回收时调用此方法。  
  **onDetach()：** 解除与Activity的绑定。在onDestroy方法之后调用。  

  **Fragment生命周期执行流程（注意红色的不是生命周期方法）：**  
  **Fragment创建：** setUserVisibleHint()->onAttach()->onCreate()->onCreateView()->onActivityCreated()->onStart()->onResume()；  
  **Fragment变为不可见状态（锁屏、回到桌面、被Activity完全覆盖）：** onPause()->onSaveInstanceState()->onStop()；  
  **Fragment变为部分可见状态（打开Dialog样式的Activity）：** onPause()->onSaveInstanceState()；  
  **Fragment由不可见变为活动状态：** onStart()->OnResume()；  
  **Fragment由部分可见变为活动状态：** onResume()；  
  **退出应用：** onPause()->onStop()->onDestroyView()->onDestroy()->onDetach()（注意退出不会调用onSaveInstanceState方法，因为是人为退出，没有必要再保存数据）；  
  **Fragment被回收又重新创建：** 被回收执行onPause()->onSaveInstanceState()->onStop()->onDestroyView()->onDestroy()->onDetach()，重新创建执行onAttach()->onCreate()->onCreateView()->onActivityCreated()->onStart()->onResume()->setUserVisibleHint()；  
  ** 横竖屏切换：** 与Fragment被回收又重新创建一样。  

3、view与viewgroup之间的关系以及区别(参考：[https://blog.csdn.net/qq941263013/article/details/82500145](https://blog.csdn.net/qq941263013/article/details/82500145 "https://blog.csdn.net/qq941263013/article/details/82500145"))
-------------

  View是所有UI组件的基类，而ViewGroup是容纳View及其派生类的容器，ViewGroup也是从View派生出来的

  **事件分发方面的区别：**  
  **事件分发机制主要有三个方法：** dispatchTouchEvent()、onInterceptTouchEvent()、onTouchEvent()  
  **(1).** ViewGroup包含这三个方法，而View则只包含dispatchTouchEvent()、onTouchEvent()两个方法，不包含onInterceptTouchEvent()。  
  **(2).** 在Action_Down的情况下，事件会先传递到最顶层的ViewGroup，调用ViewGroup的dispatchTouchEvent()，①如果ViewGroup的onInterceptTouchEvent()返回false不拦截该事件，则会分发给子View，调用子View的dispatchTouchEvent()，如果子View的dispatchTouchEvent()返回true，则调用View的onTouchEvent()消费事件。②如果ViewGroup的onInterceptTouchEvent()返回true拦截该事件，则调用ViewGroup的onTouchEvent()消费事件,接下来的Move和Up事件将由该ViewGroup直接进行处理。  
  **(3).** 子View可以调getParent().requestDisallowInterceptTouchEvent(),请求父ViewGroup不拦截事件。  
  **UI绘制方面的区别：**  
  **UI绘制主要有五个方法：** onDraw(),onLayout(),onMeasure()，dispatchDraw(),drawChild()  
  **(1).** ViewGroup包含这五个方法，而View只包含onDraw(),onLayout(),onMeasure()三个方法，不包含dispatchDraw(),drawChild()。  
  **(2).** 绘制流程：onMeasure（测量）——》onLayout（布局）——》onDraw（绘制）。  
  **(3).** 绘制按照视图树的顺序执行，视图绘制时会先绘制子控件。如果视图的背景可见，视图会在调用onDraw()之前调用drawBackGround()绘制背景。强制重绘，可以使用invalidate();  
  **(4).** 如果发生视图的尺寸变化，则该视图会调用requestLayou()，向父控件请求再次布局。如果发生视图的外观变化，则该视图会调用invalidate()，强制重绘。如果requestLayout()或invalidate()有一个被调用，框架会对视图树进行相关的测量、布局和绘制。  
  注意：视图树是单线程操作，直接调用其它视图的方法必须要在UI线程里。跨线程的操作必须使用Handler。  
  **(5).** onLayout()：对于View来说，onLayout()只是一个空实现；而对于ViewGroup来说，onLayout()使用了关键字abstract的修饰，要求其子类必须重载该方法，目的就是安排其children在父视图的具体位置。  
  **(6).** draw过程：drawBackground()绘制背景——》onDraw()对View的内容进行绘制——》dispatchDraw()对当前View的所有子View进行绘制——》onDrawScrollBars()对View的滚动条进行绘制。  

  **方法说明：**  
  **(1).** onDraw(Canvas canvas)：UI绘制最重要的方法，用于UI重绘。这个方法是所有View、ViewGroup及其派生类都具有的方法。自定义控件时，可以重载该方法，并在内容基于canvas绘制自定义的图形、图像效果。  
  **(2).** onLayout(boolean changed, int left, int top, int right, int bottom)：布局发生变化时调用此方法。这个方法是所有View、ViewGroup及其派生类都具有的方法。自定义控件时，可以重载该方法，在布局发生改变时实现特效等定制处理。  
  **(3).** onMeasure(int widthMeasureSpec, int heightMeasureSpec)：用于计算自己及所有子对象的大小。这个方法是所有View、ViewGroup及其派生类都具有的方法。自定义控件时，可以重载该方法，重新计算所有对象的大小。 MeasureSpec包含了测量的模式和测量的大小，通过MeasureSpec.getMode()获取测量模式，通过MeasureSpec.getSize()获取测量大小。mode共有三种情况： 分别为MeasureSpec.UNSPECIFIED（ View想多大就多大）, MeasureSpec.EXACTLY（默认模式，精确值模式：将layout_width或layout_height属性指定为具体数值或者match_parent。）, MeasureSpec.AT_MOST（ 最大值模式：将layout_width或layout_height指定为wrap_content。）。  
  **(4).** dispatchDraw(Canvas canvas)：ViewGroup及其派生类具有的方法，主要用于控制子View的绘制分发。自定义控件时，重载该方法可以改变子View的绘制，进而实现一些复杂的视效。  
  **(5).** drawChild(Canvas canvas, View child, long drawingTime)：ViewGroup及其派生类具有的方法，用于直接绘制具体的子View。自定义控件时，重载该方法可以直接绘制具体的子View。  

4、view的绘制流程
-------------

  当一个应用启动时，会启动一个主Activity，Android系统会根据活动的布局来对它进行绘制。绘制会从根视图ViewRoot的performTraversals（）方法开始，从上到下遍历整个视图树，每个查看控制负责绘制自己，而ViewGroup还需要负责通知自己的子查看进行绘制操作。视图操作的过程可以分为三个步骤，分别是测量（Measure），布局（Layout）和绘制（Draw）  

  **第一步：** OnMeasure()：测量视图大小。从顶层父View到子View递归调用measure方法，measure方法又回调OnMeasure。  
  **第二步：** OnLayout()：确定View位置，进行页面布局。从顶层父View向子View的递归调用view.layout方法的过程，即父View根据上一步measure子View所得到的布局大小和布局参数，将子View放在合适的位置上。  
  **第三步：** OnDraw()：绘制视图。ViewRoot创建一个Canvas对象，然后调用OnDraw()。六个步骤：①、绘制视图的背景；②、保存画布的图层（Layer）；③、绘制View的内容④、绘制View子视图，如果没有就不用；⑤、还原图层（Layer）；⑥、绘制滚动条。  

5、home键点击、后退按钮点击、页面跳转时Activity或fragment执行流程（参考：[https://blog.csdn.net/hellofengyu/article/details/78001960](https://blog.csdn.net/hellofengyu/article/details/78001960 "https://blog.csdn.net/hellofengyu/article/details/78001960")，[https://www.jianshu.com/p/c8f34229b6dc](https://www.jianshu.com/p/c8f34229b6dc "https://www.jianshu.com/p/c8f34229b6dc")）
-------------

  **(1).** 启动Activity：oncreate、onstart、onresume  
  **(2).** home键点击或跳转到其他Activity：onSaveInstanceState、onPause、onStop  
  **(3).** home键点击再打开APP：onRestart、onStart、onResume  
  **(4).** 长按home键杀死程序：onDestroy  
  **(5).** 点击后退键回到上一个页面，上一个页面：onResume  
  **(6).** 退出APP：onpause、onstop、ondestory  
  **(7).** 屏幕旋转：onSaveInstanceState 、onpause、onstop、onDestroy、oncreate、onstart、onRestoreInstanceState、onresume  
  **(8).** 屏幕旋转不销毁：在AndroidMainfest.xml中对OrientationActivity对应的<activity>配置android:configChanges="orientation",这时只会调用onConfigurationChanged    
  **(9).** fragment锁屏：Fragment 的生命周期方法：onPause（）、onSaveInstanceState（）、onStop（）。  
  **(10).** fragment锁屏后解锁：onStart（）、 onResume（）  
  **(11).** Activity 切换到 Fragment 的生命周期变化:Fragment 的生命周期变化为：onStart（）、onResume（）      

  **fragment切换：**  
  **(1).** 通过 add hide show  方式来切换 Fragment  
  Fragment1 的生命周期变化为：onCreate（）、onCreateView、onStart（）、onResume（） 回调 onHiddenChanged（） 方法  
  Fragment2 的生命周期变化为： onCreate（）、onCreateView、onStart（）、onResume（）  
  Fragment 2 再次返回到 Fragment 1：不走任何生命周期方法但是回调 onHiddenChanged（）方法  

  **(2).** 使用 replace 的方法进行切换时    
  载入Fragment 1时：   
  Fragment 1的生命周期：onCreate（）、onCreateView（）、onStart（）、onResume（）  
  切换到Fragment2时：  
  Fragment 1的生命周期：onPause（）、onStop()、onDestroyView（）、onDestroy（）  
  Fragment 2的生命周期：onCreate（）、onCreateV（）、onStart（）、onResume（）  
  Fragment 2切换回Fragment 1时：  
  Fragment2的生命周期：onPause（）、onStop()、onDestroyView（）、onDestroy（）  
  Fragment 1的生命周期：onCreate（）、onCreateV（）、onStart（）、onResume（）  
  **(3).** 使用 ViewPager 进行切换时    
  当使用 ViewPager 与 Fragment 进行切换时，Fragment 会进行预加载操作  
  所有的 Fragment 都会提前初始--->预加载；  
  初始化时 Fragment 们的生命周期：  
  Fragment 1 的生命周期：onCreate（）、onCreateView（）  
  Fragment 2 的生命周期：onCreate（）、 onCreateView（）  
  Fragment 1 切换到 Fragment 2 的生命周期：  
  Fragment 1 ：不走任何生命周期；  
  Fragment 2 ：走 setUserVisVleHint（）方法  
  切回去也是一样的  

6、图片的三级缓存
-------------

  **原理：**  
  &emsp;&emsp;当Android端需要获得数据时比如获取网络中的图片，我们首先从内存中查找（按键查找），内存中没有的再从磁盘文件或sqlite中去查找，若磁盘中也没有才通过网络获取  

  **网络缓存：**  
  &emsp;&emsp;通过Cache-Control的设置进行网络缓存  
  **内存缓存：**  
  &emsp;&emsp;内存缓存我们采用了LruCache  
  &emsp;&emsp;LruCache是android提供的一个缓存工具类，其算法是最近最少使用算法。  
  它把最近使用的对象用“强引用”存储在LinkedHashMap中，并且把最近最少使用的对象在缓存值达到预设定值之前就从内存中移除  
  **磁盘缓存**  
  &emsp;&emsp;使用的是DiskLruCache

7、LruCache：底层实现原理
-------------

  &emsp;&emsp;LruCache中Lru算法的实现就是通过LinkedHashMap来实现的。LinkedHashMap继承于HashMap，它使用了一个双向链表来存储Map中的Entry顺序关系，对于get、put、remove等操作，LinkedHashMap除了要做HashMap做的事情，还做些调整Entry顺序链表的工作。 

  &emsp;&emsp;LruCache中将LinkedHashMap的顺序设置为LRU顺序来实现LRU缓存，每次调用get(也就是从内存缓存中取图片)，则将该对象移到链表的尾端。 

  &emsp;&emsp;调用put插入新的对象也是存储在链表尾端，这样当内存缓存达到设定的最大值时，将链表头部的对象（近期最少用到的）移除。  

8、四大组件是什么？
-------------

   Activity【活动】：用于表现功能。  
   Service【服务】：后台运行服务，不提供界面呈现。  
   BroadcastReceiver【广播接收器】：用来接收广播。  
   Content Provider【内容提供商】：支持在多个应用中存储和读取数据，相当于数据库。  

9、Activity的四种启动模式对比？
-------------

   &emsp;&emsp;**Standard:** 标准的启动模式，如果需要启动一个activity就会创建该activity的实例。也是activity的默认启动模式。  
   &emsp;&emsp;**SingeTop:** 如果启动的activity已经位于栈顶，那么就不会重新创建一个新的activity实例。而是复用位于栈顶的activity实例对象。如果不位于栈顶仍旧会重新创建activity的实例对象。  
   &emsp;&emsp;**SingleTask:** 设置了singleTask启动模式的activity在启动时，如果位于activity栈中，就会复用该activity，这样的话，在该实例之上的所有activity都依次进行出栈操作，即执行对应的onDestroy()方法，直到当前要启动的activity位于栈顶。一般应用在网页的图集，一键退出当前的应用程序。  
   &emsp;&emsp;**singleInstance:** 如果使用singleInstance启动模式的activity在启动的时候会复用已经存在的activity实例。不管这个activity的实例是位于哪一个应用当中，都会共享已经启动的activity的实例对象。使用了singlestance的启动模式的activity会单独的开启一个共享栈，这个栈中只存在当前的activity实例对象。  

10、谈谈ContentProvider、ContentResolver、ContentObserver之间的关系？
-------------

   **ContentProvider：**  
   **(1).** 四大组件的内容提供者，主要用于对外提供数据  
   **(2).** 实现各个应用程序之间的（跨应用）数据共享，比如联系人应用中就使用了ContentProvider，你在自己的应用中可以读取和修改联系人的数据，不过需要获得相应的权限。其实它也只是一个中间人，真正的数据源是文件或者SQLite等  
   **(3).** 一个应用实现ContentProvider来提供内容给别的应用来操作，通过ContentResolver来操作别的应用数据，当然在自己的应用中也可以  
   **ContentResolver：**  
   **(1).** 内容解析者，用于获取内容提供者提供的数据  
   **(2).** ContentResolver.notifyChange（uri）发出消息  
   **(3).** ContentResolver.registerContentObserver()监听消息  
   **ContentObserver：**  
   **(1).** 内容监听器，可以监听数据的改变状态  
   **(2).** 目的是观察（捕捉）特定Uri引起的数据库的变化，继而做一些相应的处理，它类似于数据库技术中的触发器（Trigger），当ContentObserver所观察的Uri发生变化时，便会触发它。触发器分为表触发器、行触发器，相应地ContentObsever也分为表ContentObserver、行ContentObserver，当然这是与它所监听的Uri MIME Type有关的

11、广播的分类？
-------------

   分为有序广播和无序广播两类。  
   有序广播的优先级别使用 android:priority=""来指定,最高是1000,最低是-1000  

13、Service生命周期？
-------------

   &emsp;&emsp;service 启动方式有两种，一种是通过startService()方式进行启动，另一种是通过bindService()方式进行启动。不同的启动方式他们的生命周期是不一样.  

   &emsp;&emsp;通过startService()这种方式启动的service，生命周期是这样：调用startService() --> onCreate()--> onStartConmon()--> onDestroy()。这种方式启动的话，需要注意一下几个问题，第一：当我们通过startService被调用以后，多次在调用startService(),onCreate()方法也只会被调用一次，而onStartConmon()会被多次调用当我们调用stopService()的时候，onDestroy()就会被调用，从而销毁服务。第二：当我们通过startService启动时候，通过intent传值，在onStartConmon()方法中获取值的时候，一定要先判断intent是否为null。  

   &emsp;&emsp;通过bindService()方式进行绑定，这种方式绑定service，生命周期走法：bindService-->onCreate()-->onBind()-->unBind()-->onDestroy() 

   &emsp;&emsp;bingservice 这种方式进行启动service好处是更加便利activity中操作service，比如加入service中有几个方法，a,b ，如果要在activity中调用，在需要在activity获取ServiceConnection对象，通过ServiceConnection来获取service中内部类的类对象，然后通过这个类对象就可以调用类中的方法，当然这个类需要继承Binder对象

14、Broadcast注册方式与区别 
-------------

   此处延伸：什么情况下用动态注册Broadcast广播，注册方式主要有两种.

   **第一种是静态注册，** 也可成为常驻型广播，这种广播需要在Androidmanifest.xml中进行注册，这中方式注册的广播，不受页面生命周期的影响，即使退出了页面，也可以收到广播这种广播一般用于想开机自启动啊等等，由于这种注册的方式的广播是常驻型广播，所以会占用CPU的资源。  

   **第二种是动态注册，** 而动态注册的话，是在代码中注册的，这种注册方式也叫非常驻型广播，收到生命周期的影响，退出页面后，就不会收到广播，我们通常运用在更新UI方面。这种注册方式优先级较高。最后需要解绑，否会会内存泄露广播是分为有序广播和无序广播。  

15、HttpClient与HttpUrlConnection的区别
-------------

   此处延伸：Volley里用的哪种请求方式（2.3前HttpClient，2.3后HttpUrlConnection）

   &emsp;&emsp;首先HttpClient和HttpUrlConnection 这两种方式都支持Https协议，都是以流的形式进行上传或者下载数据，也可以说是以流的形式进行数据的传输，还有ipv6,以及连接池等功能。HttpClient这个拥有非常多的API，所以如果想要进行扩展的话，并且不破坏它的兼容性的话，很难进行扩展，也就是这个原因，Google在Android6.0的时候，直接就弃用了这个HttpClient.

   &emsp;&emsp;而HttpUrlConnection相对来说就是比较轻量级了，API比较少，容易扩展，并且能够满足Android大部分的数据传输。比较经典的一个框架volley，在2.3版本以前都是使用HttpClient,在2.3以后就使用了HttpUrlConnection。  

16、讲解一下Context
-------------

   &emsp;&emsp;Context是一个抽象基类。在翻译为上下文，也可以理解为环境，是提供一些程序的运行环境基础信息。

   &emsp;&emsp;Context下有两个子类，ContextWrapper是上下文功能的封装类，而ContextImpl则是上下文功能的实现类。而ContextWrapper又有三个直接的子类，ContextThemeWrapper、Service和Application。其中，ContextThemeWrapper是一个带主题的封装类，而它有一个直接子类就是Activity，所以Activity和Service以及Application的Context是不一样的，只有Activity需要主题，Service不需要主题。

   &emsp;&emsp;Context一共有三种类型，分别是Application、Activity和Service。这三个类虽然分别各种承担着不同的作用，但它们都属于Context的一种，而它们具体Context的功能则是由ContextImpl类去实现的，因此在绝大多数场景下，Activity、Service和Application这三种类型的Context都是可以通用的。不过有几种场景比较特殊，比如启动Activity，还有弹出Dialog。出于安全原因的考虑，Android是不允许Activity或Dialog凭空出现的，一个Activity的启动必须要建立在另一个Activity的基础之上，也就是以此形成的返回栈。而Dialog则必须在一个Activity上面弹出（除非是System Alert类型的Dialog），因此在这种场景下，我们只能使用Activity类型的Context，否则将会出错。

   &emsp;&emsp;getApplicationContext()和getApplication()方法得到的对象都是同一个application对象，只是对象的类型不一样。

   &emsp;&emsp;Context数量 = Activity数量 + Service数量 + 1 （1为Application）

17、理解Activity，View,Window三者关系
-------------

   Activity像一个工匠（控制单元），Window像窗户（承载模型），View像窗花（显示视图）LayoutInflater像剪刀，Xml配置像窗花图纸。

   **(1).** Activity构造的时候会初始化一个Window，准确的说是PhoneWindow。  
   **(2).** 这个PhoneWindow有一个“ViewRoot”，这个“ViewRoot”是一个View或者说ViewGroup，是最初始的根视图。  
   **(3).** “ViewRoot”通过addView方法来一个个的添加View。比如TextView，Button等  
   **(4).** 这些View的事件监听，是由WindowManagerService来接受消息，并且回调Activity函数。比如onClickListener，onKeyDown等。  

18、栈与队列的区别
-------------

   **(1).** 队列先进先出，栈先进后出  
   **(2).** 对插入和删除操作的"限定"。 栈是限定只能在表的一端进行插入和删除操作的线性表。 队列是限定只能在表的一端进行插入和在另一端进行删除操作的线性表。  
   **(3).** 遍历数据速度不同  

19、AIDL理解
-------------

   &emsp;&emsp;AIDL: 每一个进程都有自己的Dalvik VM实例，都有自己的一块独立的内存，都在自己的内存上存储自己的数据，执行着自己的操作，都在自己的那片狭小的空间里过完自己的一生。而aidl就类似与两个进程之间的桥梁，使得两个进程之间可以进行数据的传输，跨进程通信有多种选择，比如 BroadcastReceiver , Messenger等，但是 BroadcastReceiver 占用的系统资源比较多，如果是频繁的跨进程通信的话显然是不可取的；Messenger 进行跨进程通信时请求队列是同步进行的，无法并发执行。

20、Android内存泄露及管理
-------------

   **(1).** 内存溢出（OOM）和内存泄露（对象无法被回收）的区别。  
   **(2).** 引起内存泄露的原因  
   **(3).** 内存泄露检测工具 ------>LeakCanary  

   **内存溢出 out of memory：** 是指程序在申请内存时，没有足够的内存空间供其使用，出现out of memory；比如申请了一个integer,但给它存了long才能存下的数，那就是内存溢出。内存溢出通俗的讲就是内存不够用。内存泄露 memory leak：是指程序在申请内存后，无法释放已申请的内存空间，次内存泄露危害可以忽略，但内存泄露堆积后果很严重，无论多少内存,迟早会被占光  
   **内存泄露原因：**  
   **(1).** Handler 引起的内存泄漏。解决：将Handler声明为静态内部类，就不会持有外部类SecondActivity的引用，其生命周期就和外部类无关，如果Handler里面需要context的话，可以通过弱引用方式引用外部类  
   **(2).** 单例模式引起的内存泄漏。解决：Context是ApplicationContext，由于ApplicationContext的生命周期是和app一致的，不会导致内存泄漏  
   **(3).** 非静态内部类创建静态实例引起的内存泄漏。解决：把内部类修改为静态的就可以避免内存泄漏了  
   **(4).** 非静态匿名内部类引起的内存泄漏。解决：将匿名内部类设置为静态的。  
   **(5).** 注册/反注册未成对使用引起的内存泄漏。注册广播接受器、EventBus等，记得解绑。  
   **(6).** 资源对象没有关闭引起的内存泄漏。在这些资源不使用的时候，记得调用相应的类似close（）、destroy（）、recycler（）、release（）等方法释放。  
   **(7).** 集合对象没有及时清理引起的内存泄漏。通常会把一些对象装入到集合中，当不使用的时候一定要记得及时清理集合，让相关对象不再被引用。  

21、App启动的方式
-------------

   **冷启动：** App没有启动过或App进程被killed, 系统中不存在该App进程, 此时启动App即为冷启动。  
   **热启动：** 热启动意味着你的App进程只是处于后台, 系统只是将其从后台带到前台, 展示给用户。  
   **介于冷启动和热启动之间, 一般来说在以下两种情况下发生:**  
   **(1).** 用户back退出了App, 然后又启动. App进程可能还在运行, 但是activity需要重建。  
   **(2).** 用户退出App后, 系统可能由于内存原因将App杀死, 进程和activity都需要重启, 但是可以在onCreate中将被动杀死锁保存的状态(saved instance state)恢复。  

22、 请介绍下Android中常用的五种布局。
-------------

   ps：约束布局参考：[https://blog.csdn.net/airsaid/article/details/79052011](https://blog.csdn.net/airsaid/article/details/79052011)

   FrameLayout（框架布局），LinearLayout （线性布局），AbsoluteLayout（绝对布局），RelativeLayout（相对布局），TableLayout（表格布局），GridLayout(网格布局),ConstraintLayout(约束布局)  
   **(1).FrameLayout：** 所有东西依次都放在左上角，会重叠，这个布局比较简单，也只能放一点比较简单的东西。  
   **(2).LinearLayout：** 线性布局，每一个LinearLayout里面又可分为垂直布局（android:orientation="vertical"）和水平布局（android:orientation="horizontal" ）。当垂直布局时，每一行就只有一个元素，多个元素依次垂直往下；水平布局时，只有一行，每一个元素依次向右排列。  
   **(3).AbsoluteLayout：** 绝对布局用X,Y坐标来指定元素的位置，这种布局方式也比较简单，但是在屏幕旋转时，往往会出问题，而且多个元素的时候，计算比较麻烦。  
   **(4).RelativeLayout：** 相对布局可以理解为某一个元素为参照物，来定位的布局方式。主要属性有：相对于某一个元素android:layout_below、android:layout_toLeftOf相对于父元素的地方android:layout_alignParentLeft、android:layout_alignParentRigh；  
   **(5).TableLayout：** 表格布局，每一个TableLayout里面有表格行TableRow，TableRow里面可以具体定义每一个元素。每一个布局都有自己适合的方式，这五个布局元素可以相互嵌套应用，做出美观的界面。  
   **(6).GridLayout** 网格布局,  作为android 4.0 后新增的一个布局,与前面介绍过的TableLayout(表格布局)其实有点大同小异;不过新增了一些东东,①跟LinearLayout(线性布局)一样,他可以设置容器中组件的对齐方式;②容器中的组件可以跨多行也可以跨多列(相比TableLayout直接放组件,占一行相比较)  
   **(7).ConstraintLayout** 约束布局，ConstraintLayout 是一个 ViewGroup，它的出现是为了解决复杂布局时，布局嵌套（布局内的布局）过多的问题（嵌套布局会增加绘制界面所需的时间）。它可以根据同级视图和父布局的约束条件为每个视图定义位置，类似于 RelativeLayout 所有视图都是根据兄弟视图和父级布局之间的关系来布局的，但是与 RelativeLayout 相比，它更加灵活，更易于使用。  

23、描述一下android的系统架构
-------------

   android系统架构分从下往上为linux 内核层、运行库、应用程序框架层、和应用程序层。  
