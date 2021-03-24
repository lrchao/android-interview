### invalidate和postInvalicate区别

- 相同点：都是用来更新当前的View
- 不同点：invalide实在UI线程中刷新View，要想在非UI线程中刷新View，就要用postInvalidate，因为postInvalidate底层是使用了Handler


调用invalidate时，它会检查上一次请求的UI重绘是否完成，如果没有完成的话，那么它就什么都不做。

~~~
void invalidateInternal(int l, int t, int r, int b, boolean invalidateCache,
            boolean fullInvalidate) {
            .....
         //DRAWN和HAS_BOUNDS是否被设置为１，说明上一次请求执行的UI绘制已经完成，那么可以再次请求执行
        if ((mPrivateFlags & (PFLAG_DRAWN | PFLAG_HAS_BOUNDS)) == (PFLAG_DRAWN | PFLAG_HAS_BOUNDS)
                || (invalidateCache && (mPrivateFlags & PFLAG_DRAWING_CACHE_VALID) == PFLAG_DRAWING_CACHE_VALID)
                || (mPrivateFlags & PFLAG_INVALIDATED) != PFLAG_INVALIDATED
                || (fullInvalidate && isOpaque() != mLastIsOpaque)) {
                 ......
                 final AttachInfo ai = mAttachInfo;
                final ViewParent p = mParent;
                final Rect damage = ai.mTmpInvalRect;
                damage.set(l, t, r, b);
                p.invalidateChild(this, damage);//TODO:这是invalidate执行的主体
                .....
        }
    }
~~~
