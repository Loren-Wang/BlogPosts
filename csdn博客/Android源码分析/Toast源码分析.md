 首先，这是我第一次写源码分析，大家不喜勿喷，我只是在阐述我自己的理解，如果有出入欢迎探讨！

之所以写这篇文章是因为要写一个吐司，自定义显示时间那种，于是我就使用了toast的吐司时间设置，但是我最后才发现我工作了这么久竟然记错了，而且一错就是好几年，我也真是服了自己啊!

首先，Toast吐司的事件是固定的，默认是**短时间short为4000ms**，**长时间Long为7000ms**，这个是很容易查到的，在Toast的内部有一个**TN**内部类中定义的。

```java
private static class TN extends ITransientNotification.Stub {
 private final WindowManager.LayoutParams mParams = new WindowManager.LayoutParams();private static final int SHOW = 0;  

private static final int HIDE = 1;  
private static final int CANCEL = 2;  
final Handler mHandler;  

int mGravity;  
int mX, mY;  
float mHorizontalMargin;  
float mVerticalMargin;  


View mView;  
View mNextView;  
int mDuration;  

WindowManager mWM;  

String mPackageName;  

static final long SHORT_DURATION_TIMEOUT = 4000;  
static final long LONG_DURATION_TIMEOUT = 7000;
```

而之前我所说的记错了好几年的就是时间的设置，Toast不能自定义时间设置显示，它只能通过传参设置显示的时间是长时间还是短时间，如下设置提示：

```java
设置显示时间类型
    /**
     * Set how long to show the view for.
     * @see #LENGTH_SHORT
     * @see #LENGTH_LONG
     */
    public void setDuration(@Duration int duration) {
        mDuration = duration;
        mTN.mDuration = duration;
    }
    
普通的吐司弹窗
    /**
     * Make a standard toast that just contains a text view.
     *
     * @param context  The context to use.  Usually your {@link android.app.Application}
     *                 or {@link android.app.Activity} object.
     * @param text     The text to show.  Can be formatted text.
     * @param duration How long to display the message.  Either {@link #LENGTH_SHORT} or
     *                 {@link #LENGTH_LONG}
     *
     */
    public static Toast makeText(Context context, CharSequence text, @Duration int duration) {
        return makeText(context, null, text, duration);
    }
    

时间的注解类
    /** @hide */
    @IntDef(prefix = { "LENGTH_" }, value = {
            LENGTH_SHORT,
            LENGTH_LONG
    })
    @Retention(RetentionPolicy.SOURCE)
    public @interface Duration {}

```

关于我的错误是说完了，大家以后一定要注意啊，这点很重要，不要犯和我一样的错误了。下面再介绍一些其他的东西，在这个吐司当中，Toast本身仅仅只是一些参数的调用和初始化，最重要的就是内部的**TN**内部类，该内部类实现了吐司的显示与隐藏等。

第一步，之所以在Toast当中的构造函数以及maketask方法当中传递Looper是为了给handle使用，是消息传递使用，详细了解请看我的另外两篇文章：[https://blog.csdn.net/cmwly/article/details/94357754](安卓的消息传递机制)

[https://blog.csdn.net/cmwly/article/details/92777440](Java基础)

他的主要作用就是对于消息队列的管理，负责将消息队列当中的数据取出然后并传递出去。这个Looper最后是到了下面的位置：

```java
if (looper == null) {
                // Use Looper.myLooper() if looper is not specified.
                looper = Looper.myLooper();
                if (looper == null) {
                    throw new RuntimeException(
                            "Can't toast on a thread that has not called Looper.prepare()");
                }
            }
            mHandler = new Handler(looper, null) {
                @Override
                public void handleMessage(Message msg) {
                    switch (msg.what) {
                        case SHOW: {
                            IBinder token = (IBinder) msg.obj;
                            handleShow(token);
                            break;
                        }
                        case HIDE: {
                            handleHide();
                            // Don't do this in handleHide() because it is also invoked by
                            // handleShow()
                            mNextView = null;
                            break;
                        }
                        case CANCEL: {
                            handleHide();
                            // Don't do this in handleHide() because it is also invoked by
                            // handleShow()
                            mNextView = null;
                            try {
                                getService().cancelToast(mPackageName, TN.this);
                            } catch (RemoteException e) {
                            }
                            break;
                        }
                    }
                }
            };
```

通过上面的代码大家应该发现了这是对于吐司的显示、隐藏、取消的管理，其中最重要的就是就是显示的逻辑，隐藏和取消的没啥都还好。

```java
public void handleShow(IBinder windowToken) {
            if (localLOGV) Log.v(TAG, "HANDLE SHOW: " + this + " mView=" + mView
                    + " mNextView=" + mNextView);
            // If a cancel/hide is pending - no need to show - at this point
            // the window token is already invalid and no need to do any work.
            if (mHandler.hasMessages(CANCEL) || mHandler.hasMessages(HIDE)) {
                return;
            }
            if (mView != mNextView) {
                // remove the old view if necessary
                handleHide();
                mView = mNextView;
                Context context = mView.getContext().getApplicationContext();
                String packageName = mView.getContext().getOpPackageName();
                if (context == null) {
                    context = mView.getContext();
                }
                mWM = (WindowManager)context.getSystemService(Context.WINDOW_SERVICE);
                // We can resolve the Gravity here by using the Locale for getting
                // the layout direction
                final Configuration config = mView.getContext().getResources().getConfiguration();
                final int gravity = Gravity.getAbsoluteGravity(mGravity, config.getLayoutDirection());
                mParams.gravity = gravity;
                if ((gravity & Gravity.HORIZONTAL_GRAVITY_MASK) == Gravity.FILL_HORIZONTAL) {
                    mParams.horizontalWeight = 1.0f;
                }
                if ((gravity & Gravity.VERTICAL_GRAVITY_MASK) == Gravity.FILL_VERTICAL) {
                    mParams.verticalWeight = 1.0f;
                }
                mParams.x = mX;
                mParams.y = mY;
                mParams.verticalMargin = mVerticalMargin;
                mParams.horizontalMargin = mHorizontalMargin;
                mParams.packageName = packageName;
                mParams.hideTimeoutMilliseconds = mDuration ==
                    Toast.LENGTH_LONG ? LONG_DURATION_TIMEOUT : SHORT_DURATION_TIMEOUT;
                mParams.token = windowToken;
                if (mView.getParent() != null) {
                    if (localLOGV) Log.v(TAG, "REMOVE! " + mView + " in " + this);
                    mWM.removeView(mView);
                }
                if (localLOGV) Log.v(TAG, "ADD! " + mView + " in " + this);
                // Since the notification manager service cancels the token right
                // after it notifies us to cancel the toast there is an inherent
                // race and we may attempt to add a window after the token has been
                // invalidated. Let us hedge against that.
                try {
                    mWM.addView(mView, mParams);
                    trySendAccessibilityEvent();
                } catch (WindowManager.BadTokenException e) {
                    /* ignore */
                }
            }
        }
```

这里面有几点好处，首先它使用的Context是App的Context而不是页面实例的Context，这样避免了持有实例的引用，从而导致实例无法释放造成内存泄漏。

```java
      Context context = mView.getContext().getApplicationContext();
```

第二点就是使用的是系统级别的弹窗，使其不会手当前App实例的影响导致异常或者无法弹出：`mWM = (WindowManager)context.getSystemService(Context.WINDOW_SERVICE);`

起始Toast总结下来就以下几点：

1. 消失时间固定就两种，除非你自己手动取消；

2. 你可以自定义view显示，但是注意，**自定义view之后不用使用Toast的setText（）方法**，否则会导致崩溃；

3. Toast本身是配置显示参数的入口以及调用显示隐藏的中转站，没什么其他的了；

4. 真正的显示隐藏的处理逻辑在Toast的内部类TN中，如果想要自定义Toast，**TN**是重中之重，如果要处理的话建议使用反射区处理，当然我还没有实测，后面会实测处理试试；

剩下的起始就没啥说的了，只要对Android了解的人基本上瞅一眼就明天怎么回事了，本身这个Toast吐司就不是什么复杂的东西，比较复杂的逻辑其实并不在这个Toast当中，是后面的一些代码，但是我还没有研究到那种程度，后面慢慢来，毕竟我也不是什么大神，仅仅只是普通的开发者中的一员！！    

写这篇文章的意义一个是提醒自己犯过的错误，另外一个就是分享下我自己的理解，让大家多多交流，也没什么其他的，大家不喜勿喷。
