### LiveData的生命周期如何监听的?


- LiveData底层通过Lifecycle来监听activity或fragment的生命周期变化，在destory的时候自动移除监听，减少了内存泄漏的 发生
- 通过持有LifecycleOwner，在进行数据分发的时候获取当前的生命周期状态来决定是否分发数据改变。
- 每一次数据改变都会增加mVersion的值，防止因为生命周期的变化而不断调用同一个监听者的onDataChange方法。
- 从设计模式上来看的话livedata用到了观察者模式和装饰模式。

