当一个广播消息被Receiver监听到时，Android会调用它的onReceive()方法，并将包含消息的 Intent对象传给它处理， onReceive() 方法的执行时间不能超过10 秒，否则会导致ANR。

那么，相信很多人在这里跟我有同样的疑惑：此时直接在Broadcast Receiver中启动子线程来处理耗时任务不行吗?

这种方式不能说不行，只能说不可靠，Receiver只在onReceive方法执行时是激活状态，只要onReceive一返回，Receiver就不再是激活状态了。
由于activity可能会被用户退出，Broadcast Receiver的生命周期本身就很短，可能出现的情况是: 在子线程还没有结束的情况下，Activity已经被用户退出了，或者BroadcastReceiver已经结束了。
在Activity已经退出、BroadcastReceiver已经结束的情况下，此时它们所在的进程就变成了空进程(没有任何活动组件的进程)，系统需要内存时可能会优先终止该进程。
<b>如果宿主进程被终止，那么该进程内的所有子线程也会被中止，这样就可能导致子线程无法执行完成.。</b>

这就带来了一个问题：当响应一个广播信息的处理十分耗时的时候，那么就应该把这个处理放在一个单独的线程里去执行，来保证主线程里的其他用户交互组件能够继续运行，
而一旦这么做，当onReceive()唤起一个线程后就会马上返 回，这时就会把Receiver进程放到被终止的境地。解决这个问题的方案是在onReceive()里开始一个Service，
让这个Service去 做这件事情，那么系统就会认为这个进程里还有活动正在进行。推荐使用IntentService。
