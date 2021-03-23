### 为什么Dialog不能用Application的Context

**得到的WindowManager的实例不一样，导致token为null了**

Activity 被创建之后会调用 attach() 方法，做了这么几件事：

- 创建了 PhoneWindow 对象 mWondow
- 给当前 window 绑定 mToken
- ......

根据传入的 Context 调用 getSystemService(Context.WINDOW_SERVICE) 方法来得到 WindowManager 对象 mWindowManager ，最终会通过 mWindowManager.addWindow() 来显示 dialog 。      

#### 如果传入的 Context 是 Activity，返回的是在 Activity.attach() 方法中创建的 mWindowManager 对象，这个时候 mToken 也已经绑定。

~~~java
> Activity.java

@Override
public Object getSystemService(@ServiceName @NonNull String name) {
    ......

    if (WINDOW_SERVICE.equals(name)) {
        return mWindowManager; // 在 attach() 方法中已经实例化
    } else if (SEARCH_SERVICE.equals(name)) {
        ensureSearchManager();
        return mSearchManager;
    }
    return super.getSystemService(name);
}
~~~

#### 如果传入的 Context 是 Application，最终调用的是父类 ContextImpl 的方法。

~~~java
@Override
public Object getSystemService(String name) {
    return SystemServiceRegistry.getSystemService(this, name);
}
~~~

参考：
- https://juejin.cn/post/6867390363020361742
