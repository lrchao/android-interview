### Okhttp之addInterceptor与addNetworkInterceptor的区别

#### addInterceptor（应用拦截器）：
1，不需要担心中间过程的响应,如重定向和重试.   
2，总是只调用一次,即使HTTP响应是从缓存中获取.    
3，观察应用程序的初衷. 不关心OkHttp注入的头信息如: If-None-Match.   
4，允许短路而不调用 Chain.proceed(),即中止调用.   
5，允许重试,使 Chain.proceed()调用多次.   

#### addNetworkInterceptor（网络拦截器）：
1，能够操作中间过程的响应,如重定向和重试.    
2，当网络短路而返回缓存响应时不被调用.    
3，只观察在网络上传输的数据.   
4，携带请求来访问连接.    
