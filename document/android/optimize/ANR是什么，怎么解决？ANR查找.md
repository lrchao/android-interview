### ANR是什么，怎么解决？ANR查找?

#### 是什么

1. 触摸事件5秒未响应
2. 广播10秒未处理完
3. ServiceTimeout 20秒未处理完
4. 线程死锁

#### 查找

- 开源框架ANR-WatchDog https://github.com/SalomonBrys/ANR-WatchDog
- Trace.txt分析

#### 解决

- 使用子线程处理耗时操作，例如：网络、数据库、I/0
- UI线程只做View的操作
- 使用Handler
