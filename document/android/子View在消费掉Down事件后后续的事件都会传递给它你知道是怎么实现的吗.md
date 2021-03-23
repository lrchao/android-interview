### 子View在消费掉Down事件后，后续的事件都会传递给它，你知道是怎么实现的吗?

其实android事件分发核心是在viewgroup的dispatchTouchEvent的action_down过程中找到`mFirstTouchTarget是否为空`，mFirstTouchTarget 是一个 TouchTarget 的链表
通过`反序遍历子view的dispatchTouchEvent的方法`，
如果发现有一个子view的dispatchTouchEvent方法返回true，那么mFirstTouchTarget就不为空，否则为空。
如果mFirstTouchTarget不为空，那么action_move和action_up才会往下传递，如果在action_move和action_up过程中有viewgroup拦截了事件，
则此时先向子view的dispatchTouchEvent传递一个action_cancel，并且将mFirstTouchTarget至为null，
所以此时action_move和action_up只会走viewgroup的dispatchTouchEvent和onTouchEvent；
如果mFirstTouchTarget在action_down过程中就已经null的话，则从action_down一直向上层view传递，不会有后续的action_move和action_up了。

- 判断是否需要拦截 —> 主要是根据 onInterceptTouchEvent 方法的返回值来决定是否拦截；
- 在 DOWN 事件中将 touch 事件分发给子 View —> 这一过程如果有子 View 捕获消费了 touch 事件，会对 mFirstTouchTarget 进行赋值；
- 最后一步，DOWN、MOVE、UP 事件都会根据 mFirstTouchTarget 是否为 null，决定是自己处理 touch 事件，还是再次分发给子 View。
- DOWN 事件是事件序列的起点；决定后续事件由谁来消费处理；
- mFirstTouchTarget 的作用：记录捕获消费 touch 事件的 View，是一个链表结构；
- CANCEL 事件的触发场景：当父视图先不拦截，然后在 MOVE 事件中重新拦截，此时子 View 会接收到一个 CANCEL 事件。
- 如果一个事件最后所有的 View 都不处理的话，最终回到 Activity 的 onTouchEvent 方法里面来。

