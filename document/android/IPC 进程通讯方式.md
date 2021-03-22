Intent 、Bundle ：要求传递数据能被序列化，实现 Parcelable、Serializable ，适用于四大组件通信。

文件共享 ：适用于交换简单的数据实时性不高的场景。

AIDL：AIDL 接口实质上是系统提供给我们可以方便实现 BInder 的工具。

Messenger：基于 AIDL 实现，服务端串行处理，主要用于传递消息，适用于低并发一对多通信。

ContentProvider：基于 Binder 实现，适用于一对多进程间数据共享。（通讯录 短信 等）

Socket：TCP、UDP，适用于网络数据交换
