### Looper.loop为什么不会卡死.md

为了app不挂掉，就要保证主线程一直运行存在，使用死循环代码阻塞在msgQueue.next()中的nativePollOnce()方法里 ，主线程就会挂起休眠释放cpu，线程就不会退出
。Looper死循环之前，在ActivityThread.main()中就会创建一个 Binder 线程（ApplicationThread），接收系统服务AMS发送来的事件。
当系统有消息产生（其实系统每 16ms 会发送一个刷新 UI 消息唤醒）会通过epoll机制 向pipe管道写端写入数据 就会发送消息给 looper 接收到消息后处理事件，保证主线程的一直存活。
只有在主线程中处理超时才会让app崩溃 也就是ANR。
