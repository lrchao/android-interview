### Window,Activity,DecorView,ViewRootImpl之间的关系

#### Activity
不负责控制视图，只是控制生命周期和处理事件，真正控制视图的是Window,Activity中含有一个Window，Window才是真正代表一个窗口

#### Window
视图的承载器，内部持有DecorView,而DecorView是View的根布局，Window是一个抽象类，真正的实现类是PhoneWindow，PhoneWindow有个内部类DecorView,
通过其来加载R.layout.activity_main。Window通过WindowManager将DecorView加载其中，并将DecorView交给ViewRoot，进行视图的绘制及其他交互

#### DecorView
是FrameLayout的子类，是android的根视图，相当于顶级View，一般来说内部包含竖直方向LinearLayout,在linearlayout中含有三部分，
上面是ViewStub,延迟加载的视图，中间是标题栏，下面是内容栏，就是我们熟悉的android.R.id.content

#### ViewRoot
所有View的绘制及事件分发交互都是通过它来进行的，有个真正的实现类ViewRootImpl,它是链接WindowManagerService和DecorView的纽带，View的测量，布局，
绘制都是通过它来实现的，所以我们常见的事件分发真正的过程是

`硬件->ViewRootImpl->DecorView->PhoneWindow->Activity->ViewGroup->View`


原文链接：https://blog.csdn.net/tiangaopan/article/details/106238414
