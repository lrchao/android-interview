### OkHttp框架中都用到了哪些设计模式

- Builder模式（OkhttpClient）
- 责任链模式（拦截器）
- 简单工厂（CacheStrategy#Factory）
- 享元模式（Dispatcher的线程池中，不限量的线程池实现了对象复用）
- 策略模式（在CacheInterceptor中，在响应数据的选择中使用了策略模式，选择缓存数据还是选择网络访问。）
