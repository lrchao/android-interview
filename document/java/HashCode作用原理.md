### HashCode作用原理

 1. HashCode的存在主要是用于查找的快捷性，如Hashtable，HashMap等，HashCode经常用于确定对象的存储地址；
 2. 如果两个对象相同， equals方法一定返回true，并且这两个对象的HashCode一定相同；
 3. 两个对象的HashCode相同，并不一定表示两个对象就相同，即equals()不一定为true，只能说明这两个对象在一个散列存储结构中。
 4. 如果对象的equals方法被重写，那么对象的HashCode也尽量重写
