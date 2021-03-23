不能

不能，MessageQueue同步障碍机制： 可以发现就是把一条Message，注意这个Message是没有设置target的，整个消息循环唯一一处不设置回调的target(hander)，
因为这个即使标志了同步障碍消息，也是不需要handler来pushMessage到队列中，直接手动循环移动链表插入到合适time的Message之后的即可。  

scheduleTraversals中设置了同步障碍消息，就是相当于在MessageQueue中插入了一个Message，并且是在onResume之后插入的，
所以在onResume中handler.post（Runnable）之后，这个消息会在同步障碍Message之前，会先被执行，这个时候依然没有刷新绘制界面，待查询到同步障碍Message时候，会等待下个异步Message（刷新Message）出现。   

所以在onResume中handler.post（Runnable）是Ui操作失效的。

#### 1. Activity生命周期启动流程

在Activity可见之后，紧接着就是要触发绘制界面了，会走到handleResumeActivity方法，会performResumeActivity调用activity的onResume方法     
handleResumeActivity -》 performResumeActivity -》activity的onResume方法     
// 关键函数 添加到window触发ui测量、布局、绘制        
wm.addView(decor, l);

#### 2. Message消息发送和执行原理机制

#### 3. UI绘制刷新触发原理机制

ViewRootImpl.class -》setView() -》 requestLayout()

~~~java
@Override
public void requestLayout() {
    if (!mHandlingLayoutInLayoutRequest) {
        checkThread();
        mLayoutRequested = true;
        //安排刷新请求
        scheduleTraversals();
    }
｝
final TraversalRunnable mTraversalRunnable = new TraversalRunnable();
final class TraversalRunnable implements Runnable {
    @Override
    public void run() {
        doTraversal();
    }
}  
void scheduleTraversals() {
     //一个刷新周期只执行一次即可，屏蔽其他的刷新请求
    if (!mTraversalScheduled) {
        mTraversalScheduled = true;
        //设置同步障碍Message
        mTraversalBarrier = mHandler.getLooper().getQueue().postSyncBarrier();
        //屏幕刷新信号VSYNC 监听回调把mTraversalRunnable（执行doTraversal()） push到主线程了且是个异步Message会优先得到执行 ，具体看下Choreographer的实现
        mChoreographer.postCallback(
                Choreographer.CALLBACK_TRAVERSAL, mTraversalRunnable, null);
        if (!mUnbufferedInputDispatch) {
            scheduleConsumeBatchedInput();
        }
        notifyRendererOfFramePending();
        pokeDrawLockIfNeeded();
    }
}
void doTraversal() {
    if (mTraversalScheduled) {
        mTraversalScheduled = false;
        //移除同步障碍Message
        mHandler.getLooper().getQueue().removeSyncBarrier(mTraversalBarrier);
        if (mProfile) {
            Debug.startMethodTracing("ViewAncestor");
        }
        //真正执行decorView的绘制
        performTraversals();
        if (mProfile) {
            Debug.stopMethodTracing();
            mProfile = false;
        }
    }
}
private void performTraversals() {
    // cache mView since it is used so much below...
    final View host = mView;
    .............
    Rect frame = mWinFrame;
    if (mFirst) {
        mFullRedrawNeeded = true;
        mLayoutRequested = true;
        .......
        mAttachInfo.mUse32BitDrawingCache = true;
        mAttachInfo.mHasWindowFocus = false;
        mAttachInfo.mWindowVisibility = viewVisibility;
        mAttachInfo.mRecomputeGlobalAttributes = false;
       //performTraversals 第一次调用时候decorView dispatch mAttachInfo变量
        host.dispatchAttachedToWindow(mAttachInfo, 0);
        mAttachInfo.mTreeObserver.dispatchOnWindowAttachedChange(true);
        dispatchApplyInsets(host);
        desiredWindowWidth = frame.width();
        desiredWindowHeight = frame.height();
        if (desiredWindowWidth != mWidth || desiredWindowHeight != mHeight) {
            if (DEBUG_ORIENTATION) Log.v(TAG,
                    "View " + host + " resized to: " + frame);
            mFullRedrawNeeded = true;
            mLayoutRequested = true;
            windowSizeMayChange = true;
        }
    }
   ...................
   //会根据状态判断是否执行，对mVIew（decorView）执行view的测量、布局、绘制
   perforMeasure()
   perforLayout()
   perforDraw()
   mFirst=false;
}
~~~


#### 4. MessageQueue同步障碍机制

MessageQueue同步障碍机制： 可以发现就是把一条Message，注意这个Message是没有设置target的，
整个消息循环唯一一处不设置回调的target(hander)，
因为这个即使标志了同步障碍消息，也是不需要handler来pushMessage到队列中，
直接手动循环移动链表插入到合适time的Message之后的即可。

~~~java
Message next() {
    final long ptr = mPtr;
    if (ptr == 0) {
        return null;
    }
    int pendingIdleHandlerCount = -1; // -1 only during first iteration
    int nextPollTimeoutMillis = 0;
    for (;;) {
        if (nextPollTimeoutMillis != 0) {
            Binder.flushPendingCommands();
        }
        //查询是否有下一个消息，没有就阻塞
        nativePollOnce(ptr, nextPollTimeoutMillis);
        synchronized (this) {
            final long now = SystemClock.uptimeMillis();
            Message prevMsg = null;
            Message msg = mMessages;
            //关键地方，首先识别 msg.target == null情况就是同步障碍消息，如果该消息是同步障碍消息的话，就会循环查询下一个消息是否是isAsynchronous状态，异步Message，专门给刷新UI消息使用的
            if (msg != null && msg.target == null) {
                // Stalled by a barrier.  Find the next asynchronous message in the queue.
                do {
                    prevMsg = msg;
                    msg = msg.next;
                } while (msg != null && !msg.isAsynchronous());
            }
            //如果查到异步消息或者没有设置同步障碍的消息，直接返回执行
            if (msg != null) {
                if (now < msg.when) {
                    // Next message is not ready.  Set a timeout to wake up when it is ready.
                    nextPollTimeoutMillis = (int) Math.min(msg.when - now, Integer.MAX_VALUE);
                } else {
                    // Got a message.
                    mBlocked = false;
                    if (prevMsg != null) {
                        prevMsg.next = msg.next;
                    } else {
                        mMessages = msg.next;
                    }
                    msg.next = null;
                    if (DEBUG) Log.v(TAG, "Returning message: " + msg);
                    msg.markInUse();
                    return msg;
                }
            } else {
                // No more messages.
                nextPollTimeoutMillis = -1;
            }
         ........省略.................
      nextPollTimeoutMillis = 0;
    }
}
~~~

- https://blog.csdn.net/anhenzhufeng/article/details/88670194
