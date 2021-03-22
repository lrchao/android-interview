### OkHttp对于网络请求都有哪些优化.md

- 通过ConnectionPool来减少请求延时.可以有5个空闲的connection并保持5分钟。相同IP address公用一个connection。长链接策略
- 无缝支持GZIP来减少数据流量
- 以从很多常用的连接问题中自动恢复
如果你的服务有多个IP地址，当第一个IP请求失败时，OkHttp会交替尝试你配置的其他IP，OkHttp使用现代TLS技术(SNI, ALPN)初始化新的连接，当握手失败时会回退到TLS 1.0。
- 缓存响应数据来减少重复的网络请求
OKHttp默认只支持get请求的缓存
通过字段 Expires 与 Cache-Control 标识

~~~java
public final class HttpEngine {

  //发送请求
  public void sendRequest() throws RequestException, RouteException, IOException {
    if (cacheStrategy != null) return; // Already sent.
    if (httpStream != null) throw new IllegalStateException();
    //根据用户请求得到实际的网络请求
    Request request = networkRequest(userRequest);
    //这里InternalCache就是对Cache的封装，它的实现在Cache的internalCache中。
    InternalCache responseCache = Internal.instance.internalCache(client);
    //通过Cache的get方法查找缓存响应
    Response cacheCandidate = responseCache != null
        ? responseCache.get(request)
        : null;

    long now = System.currentTimeMillis();
    //构造缓存策略，然后进行策略判断
    cacheStrategy = new CacheStrategy.Factory(now, request, cacheCandidate).get();
    //策略判定后的网络请求和缓存响应
    networkRequest = cacheStrategy.networkRequest;
    cacheResponse = cacheStrategy.cacheResponse;
    
    if (responseCache != null) {
      //使用缓存响应的话，记录一下使用记录
      responseCache.trackResponse(cacheStrategy);
    }

    if (cacheCandidate != null && cacheResponse == null) {
      //使用网络请求，但是之前又有缓存的话，要关闭缓存，释放资源
      closeQuietly(cacheCandidate.body()); // The cache candidate wasn't applicable. Close it.
    }

    // If we're forbidden from using the network and the cache is insufficient, fail.
    if (networkRequest == null && cacheResponse == null) {
      //强制使用缓存，又找不到缓存，就报不合理请求响应了
      userResponse = new Response.Builder()
          .request(userRequest)
          .priorResponse(stripBody(priorResponse))
          .protocol(Protocol.HTTP_1_1)
          .code(504)
          .message("Unsatisfiable Request (only-if-cached)")
          .body(EMPTY_BODY)
          .build();
      return;
    }
    
    //上面情况处理之后，就是使用缓存返回，还是网络请求的情况了

    // If we don't need the network, we're done.
    if (networkRequest == null) {
      //使用缓存返回响应
      userResponse = cacheResponse.newBuilder()
          .request(userRequest)
          .priorResponse(stripBody(priorResponse))
          .cacheResponse(stripBody(cacheResponse))
          .build();
      userResponse = unzip(userResponse);
      return;
    }

    //使用网络请求
    //下面就是网络请求流程了，略
    ...
  }  
}
~~~
