### View.post(Runnable)为什么可以获取到View的宽高


其实我们的 Message 并没有立即被执行，因为此时主线程的 Handler 正在执行的 Message 是 TraversalRunnable，而 performMeaure() 方法也是在该 Message 中被执行，
所以排队等到主线程的 Handler 执行到我们 post 的 Message 时，View 的宽高已经测量完毕，因此我们也就很自然的能够获取到 View 的宽高     

布局测量的handler是ViewRootImpl的，和View.post()消息的hander是同一个

#### 总结

根据 ViewRootImpl 是否已经创建，View.post() 会执行不同的逻辑。如果 ViewRootImpl 已经创建，即 mAttachInfo 已经初始化，直接通过 Handler 发送消息来执行任务。
如果 ViewRootImpl 未创建，即 View 尚未开始绘制，会将任务保存为 HandlerAction，暂存在队列 HandlerActionQueue 中，
等到 View 开始绘制，执行 performTraversal() 方法时，在 dispatchAttachedToWindow() 方法中通过 Handler 分发 HandlerActionQueue 中暂存的任务。

> 首先，你要知道 performTraversals() 是在主线程消息队列的一次消息处理过程中执行的，
而 dispatchAttachedToWindow() 间接调用的 mRunQueue.executeActions() 发送的任务也是通过 Handler 发送到主线程消息队列的，
那么它的执行就一定在这次的 performTraversals() 方法执行完之后。所以，在这里获取 View 的宽高是完全没有问题的。


#### 还可以怎么获取视图宽高？

除了通过 View.post() 获取视图宽高之外，还有两种比较推荐的方式。

第一种，onWindowFocusChanged() 。

~~~java
override fun onWindowFocusChanged(hasFocus: Boolean) {  
    super.onWindowFocusChanged(hasFocus)
    if (hasFocus){
        ...
    }
}
~~~

第二种，OnGlobalLayoutListener 。

~~~java
binding.dialog.viewTreeObserver.addOnGlobalLayoutListener(object : ViewTreeObserver.OnGlobalLayoutListener{
    override fun onGlobalLayout() {
        binding.dialog.viewTreeObserver.removeOnGlobalLayoutListener(this)
        ...
    }
})
~~~


- https://juejin.cn/post/6895735092438630407



