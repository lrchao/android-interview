### Handler如果没有消息处理是阻塞的还是非阻塞的

// 下面这个方法有可能会产生阻塞，主要是通过native层的epoll机制，监听文件描述符的写入事件实现的。 

// -1：一直阻塞；0：不阻塞；n>0：最多阻塞n秒 nativePollOnce(ptr, nextPollTimeoutMillis);
