### IdleHandler是什么？怎么使用，能解决什么问题？

- IdleHandler 是在消息队列当前无可用消息的时候发生作用的，如果你想在消息队列空暇时做一些处理，那么你就可以在当前线程的消息队列中添加 IdleHandler，并重写它的 queueIdle() 函数。

queueIdle() 方法如果返回 false，那么这个 IdleHandler 只会执行一次，执行完后会从队列中删除，如果返回 true，那么执行完后不会被删除，
`只要执行 MessageQueue.next 时消息队列中没有可执行的消息，即为空闲时间，那么 IdleHandler 队列中的 IdleHandler 还会继续被执行`。

作用：   
做一些优化操作，延迟加载之类



