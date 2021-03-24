### 说说View/ViewGroup的绘制流程

View的绘制流程是从`ViewRoot的performTraversals`开始的，它经过measure，layout，draw三个过程最终将View绘制出来。
performTraversals会依次调用performMeasure，performLayout，performDraw三个方法，他们会依次调用measure，layout，draw方法，然后又调用了onMeasure，onLayout，dispatchDraw

#### measure ：
对于自定义的单一view的测量，只需要根据父 view 传递的MeasureSpec进行计算大小

对于ViewGroup的测量，一般要重写onMeasure方法，在onMeasure方法中，父容器会对所有的子View进行Measure，
子元素又会作为父容器，重复对它自己的子元素进行Measure，这样Measure过程就从DecorView一级一级传递下去了，
也就是要遍历所有子View的的尺寸，最终得出出总的viewGroup的尺寸。Layout和Draw方法也是如此。

#### layout :
根据 measure 子 View 所得到的布局大小和布局参数，将子View放在合适的位置上。对于自定义的单一view，计算本身的位置即可。

对于ViewGroup来说，需要重写onlayout方法。除了计算自己View的位置，还需要确定每一个子View在父容器的位置以及子view的宽高（getMeasuredWidth和getMeasuredHeight），
最后调用所有子view的layout方法来设定子view的位置。

#### draw ：
把 View 对象绘制到屏幕上。    

draw（）会依次调用四个方法：

1）drawBackground()，根据在 layout 过程中获取的 View 的位置参数，来设置背景的边界。

2）onDraw()，绘制View本身的内容，一般自定义单一view会重写这个方法，实现一些绘制逻辑。

3） dispatchDraw()，绘制子View

4）onDrawScrollBars(canvas)，绘制装饰，如 滚动指示器、滚动条、和前景.






参考：
- https://blog.csdn.net/huahuahua333686/article/details/111473046
