### 为什么service里面startActivity抛异常？activity不会

1）可以看到，其实通过Activity和ContextImpl类启动Activity并无本质不同，他们都通过<b>Instrumentation这个辅助类调用到了ActivityManagerNative的方法。</b>

2）只是Activity不会去检查标记，所以并不会抛出异常。
