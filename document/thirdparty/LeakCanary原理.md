### LeakCanary 原理

通过 registerActivityLifecycleCallbacks 监听Activity或者Fragment 销毁时候的生命周期。       
（如果不想那个对象被监控则通过 AndroidExcludedRefs 枚举，避免被检测）

~~~java
public void watch(Object watchedReference, String referenceName) {

  if (this == DISABLED) {
   return;
  }
  checkNotNull(watchedReference, "watchedReference");
  checkNotNull(referenceName, "referenceName");
  final long watchStartNanoTime = System.nanoTime();
  String key = UUID.randomUUID().toString();
  retainedKeys.add(key);
  final KeyedWeakReference reference =
     new KeyedWeakReference(watchedReference, key, referenceName, queue);

  ensureGoneAsync(watchStartNanoTime, reference);
}
~~~

然后通过弱引用和引用队列监控对象是否被回收。（弱引用和引用队列ReferenceQueue联合使用时，如果弱引用持有的对象被垃圾回收，Java虚拟机就会把这个弱引用加入到与之关联的引用队列中。即 KeyedWeakReference持有的Activity对象如果被垃圾回收，该对象就会加入到引用队列queue）

~~~java
void waitForIdle(final Retryable retryable, final int failedAttempts) {

  // This needs to be called from the main thread.
  Looper.myQueue().addIdleHandler(new MessageQueue.IdleHandler() {
    @Override public boolean queueIdle() {
      postToBackgroundWithDelay(retryable, failedAttempts);
      return false;
    }
  });
}
~~~

IdleHandler，就是当主线程空闲的时候，如果设置了这个东西，就会执行它的queueIdle()方法，所以这个方法就是在onDestory以后，一旦主线程空闲了，就会执行一个延时五秒的子线程任务，任务：检测到未被回收则主动 gc ，然后继续监控，如果还是没有回收掉，就证明是内存泄漏了。通过抓取 dump文件，在使用 第三方 HAHA 库 分析文件，获取到到达泄露点最近的线路，通过 启动另一个进程的 DisplayLeakService 发送通知 进行消息的展示。


