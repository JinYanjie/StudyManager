fragment+viewpager中livedata移除引发的问题

1、Viewpager 线

2、Livedata 线

```java
    protected void setValue(T value) {
        assertMainThread("setValue");
        mVersion++;
        mData = value;
        dispatchingValue(null);
    }

dispatchingValue(null);
   >>>>> considerNotify(iterator.next().getValue());

    private void considerNotify(ObserverWrapper observer) {
        if (!observer.mActive) {
            return;
        }
        if (!observer.shouldBeActive()) {
            observer.activeStateChanged(false);
            return;
        }
        if (observer.mLastVersion >= mVersion) {
            return;
        }
        observer.mLastVersion = mVersion;
        observer.mObserver.onChanged((T) mData);
    }
  
        @Override
        public void onStateChanged(@NonNull LifecycleOwner source,
                @NonNull Lifecycle.Event event) {
            if (mOwner.getLifecycle().getCurrentState() == DESTROYED) {
                removeObserver(mObserver);
                return;
            }
            activeStateChanged(shouldBeActive());
        }
>>>>>>>>>>>>>
    mOwner.getLifecycle().getCurrentState() == DESTROYED
       removeObserver(mObserver);


Fragment:
    void performDestroyView() {
        if (mView != null) {
            mViewLifecycleRegistry.handleLifecycleEvent(Lifecycle.Event.ON_DESTROY);
        }
        if (mChildFragmentManager != null) {
            mChildFragmentManager.dispatchDestroyView();
        }
        mState = CREATED;
        mCalled = false;
        onDestroyView();
        LoaderManager.getInstance(this).markForRedelivery();
        mPerformedCreateView = false;
    }
viewPager:
  mAdapter.destroyItem(this, ii.position, ii.object);


```

3、Fragment线