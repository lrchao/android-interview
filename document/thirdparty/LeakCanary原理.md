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



