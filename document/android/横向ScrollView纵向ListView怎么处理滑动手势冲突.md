### 横向ScrollView纵向ListView怎么处理滑动手势冲突

在ViewGroup中有个方法叫`requestDisallowInterceptTouchEvent(boolean disallowIntercept)`，    
这个方法可以用来控制该ViewGroup是否截断点击事件

https://blog.csdn.net/xiaohanluo/article/details/52130923
