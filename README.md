# android-interview
Android面试题

## Data Structure
- [ArrayList、LinkedList、HashMap底层实现](document/datastructure/ArrayList、LinkedList、HashMap底层实现.md)
- [Java中中文字符和英文字符的大小分别多少](document/datastructure/Java中中文字符和英文字符的大小分别多少.md)
- [int float double多少位](document/datastructure/int_float_double多少位.md)

## Java
- [Java GC 的几种算法](document/java/JavaGC.md)
- [volatile的使用及其原理](document/java/volatile的使用及其原理.md)
- [Synchronized原理](document/java/Synchronized原理.md)
- [序列化原理](document/java/序列化原理.md)
- [Java中的几种引用类型](document/java/Java中的几种引用类型.md)
- [MD5和Base64](document/java/MD5和Base64.md)
- [HashCode作用原理](document/java/HashCode作用原理.md)
- [Java类的加载过程是什么](document/java/Java类的加载过程是什么.md)
- [synchronized锁的四种状态](document/java/synchronized锁的四种状态.md)
- [ClassLoader 的双亲委派机制](document/java/ClassLoader的双亲委派机制.md)

## Android
- [为什么不能在BroadcastReceiver中开启子线程](document/android/为什么不能在BroadcastReceiver中开启子线程.md)
- [为什么service里面startActivity抛异常？activity不会](document/android/为什么service里面startActivity抛异常？activity不会.md)
- [请描诉Activity的启动流程，从点击图标开始](document/android/请描诉Activity的启动流程，从点击图标开始.md)
- [Android中startService和bindService的区别](document/android/Android中startService和bindService的区别.md)
- [Activity启动模式](document/android/Activity启动模式.md)
- [IPC 进程通讯方式](document/android/IPC进程通讯方式.md)
- [Looper.loop 为什么不会卡死](document/android/Looper.loop为什么不会卡死.md)
- [android系统中哪些设计模式？](document/android/android系统中哪些设计模式.md)
- [APK组成](document/android/APK组成.md)
- [打包流程](document/android/打包流程.md)
- [Bitmap压缩，质量100%与90%的区别？](document/android/Bitmap压缩质量100与90的区别.md)
- [Activity中为什么要用bundle来传递数据，而不用HashMap来代替？](document/android/Activity中为什么要用bundle来传递数据，而不用HashMap来代替.md)
- [描述下Binder机制原理？](document/android/描述下Binder机制原理.md)
- [Binder线程池的工作过程是什么样？](document/android/Binder线程池的工作过程是什么样.md)
- [Handler怎么进行线程通信，原理是什么？](document/android/Handler怎么进行线程通信，原理是什么.md)
- [Handler如果没有消息处理是阻塞的还是非阻塞的？](document/android/Handler如果没有消息处理是阻塞的还是非阻塞的.md)
- [为什么Intent不能传递大数据](document/android/为什么Intent不能传递大数据.md)
- [LiveData的生命周期如何监听的?](document/android/LiveData的生命周期如何监听的.md)

## Android UI
- [子 View 在消费掉 Down 事件后，后续的事件都会传递给它，你知道是怎么实现的吗?](document/android/子View在消费掉Down事件后后续的事件都会传递给它你知道是怎么实现的吗.md)
- [横向 ScrollView、纵向 ListView 怎么处理滑动手势冲突](document/android/横向ScrollView纵向ListView怎么处理滑动手势冲突.md)
- [为什么系统不允许在子线程中访问UI？](document/android/为什么系统不允许在子线程中访问UI.md)
- [说说View/ViewGroup的绘制流程](document/android/说说ViewViewGroup的绘制流程.md)
- [说说你理解的MeasureSpec](document/android/说说你理解的MeasureSpec.md)
- [在onResume中handler.postRunnable可以测量宽高么](document/android/在onResume中handler.postRunnable可以测量宽高么.md)
- [View.post(Runnable) 为什么可以获取到 View 的宽高？](document/android/View.post(Runnable)为什么可以获取到View的宽高.md)
- [事件冲突怎么解决？](document/android/事件冲突怎么解决.md)
- [为什么Dialog不能用Application的Context？](document/android/为什么Dialog不能用Application的Context.md)
- WebView如何做资源缓存？
- [invalidate() 和 postInvalicate() 区别](document/android/invalidate和postInvalicate区别.md)
- [如何实现Activity窗口快速变暗](document/android/如何实现Activity窗口快速变暗.md)
- [自定义View执行invalidate()方法,为什么有时候不会回调onDraw](document/android/自定义View执行invalidate方法为什么有时候不会回调onDraw.md)
- 如何通过WindowManager添加Window(代码实现)？
- [WindowMangerService中token到底是什么？有什么区别](document/android/WindowMangerService中token到底是什么.md)
- [Window,Activity,DecorView,ViewRootImpl之间的关系](document/android/WindowActivityDecorViewViewRootImpl之间的关系.md)
- [RecyclerView的回收复用机制](document/android/RecyclerView的回收复用机制.md)
- [IdleHandler是什么？怎么使用，能解决什么问题？](document/android/IdleHandler是什么？怎么使用，能解决什么问题.md)

## Network

- [HTTP 与 HTTPS 区别](document/network/HTTP与HTTPS区别.md)
- [SSL四次握手过程](document/network/SSL四次握手过程.md)

## Third Party

- [LeakCanary原理](document/thirdparty/LeakCanary原理.md)
- [OkHttp原理](document/thirdparty/OkHttp原理.md)
- [OkHttp对于网络请求都有哪些优化](document/thirdparty/OkHttp对于网络请求都有哪些优化.md)
- [OkHttp框架中都用到了哪些设计模式](document/thirdparty/OkHttp框架中都用到了哪些设计模式.md)
- [Okhttp之addInterceptor与addNetworkInterceptor的区别](document/thirdparty/Okhttp之addInterceptor与addNetworkInterceptor的区别.md)
- [glide原理]
- [LiveDataBus原理]
- [glide原理]

## Experience
- [实际工作中用到哪些设计模式](document/experience/实际工作中用到哪些设计模式.md)
- 依赖注入是什么？用过哪些库？是否使用过
- [内存泄漏的场景](document/experience/内存泄漏的场景.md)

