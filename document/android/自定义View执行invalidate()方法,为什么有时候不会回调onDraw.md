自定义一个view时，重写onDraw。

调用view.invalidate(),会触发onDraw和computeScroll()。前提是该view被附加在当前窗口上

view.postInvalidate(); //是在非UI线程上调用的

自定义一个ViewGroup，重写onDraw。

onDraw可能不会被调用，原因是需要先设置一个背景(颜色或图)。

表示这个group有东西需要绘制了，才会触发draw，之后是onDraw。

因此，一般直接重写dispatchDraw来绘制viewGroup

自定义一个ViewGroup

dispatchDraw会调用drawChild
