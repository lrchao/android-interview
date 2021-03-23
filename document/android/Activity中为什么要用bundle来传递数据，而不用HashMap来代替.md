### Activity中为什么要用bundle来传递数据，而不用HashMap来代替

1.**ArrayMap适合于小数据量操作**，如果在数据量比较大的情况下，它的性能将退化。HashMap内部则是数组+链表结构， 
所以在数据量较少的时候，HashMap的Entry Array比ArrayMap占用更多的内存。而在Activity中我们使用Bundle的场景大多数为小数据量。所以使用ArrayMap实现更合适。

2.Android中如果使用Intent来携带数据的话，需要数据是基本类型或者是可序列化类型，     
**Bundle使用Parcelable进行序列化**，而HashMap则是使用Serializable进行序列化。      
在Android中Parcelable比Serializable性能要高。

