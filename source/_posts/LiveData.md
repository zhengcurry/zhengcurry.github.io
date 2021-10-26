---
title: LiveData
date: 2021-10-25 13:40:08
tags: Android
categories: Android

---

**LiveData源码的注释翻译如下：**

> LiveData 是一个可以在给定生命周期内观察到的数据持有者类。 这意味着Observer可以与LifecycleOwner成对添加，并且仅当配对的 LifecycleOwner 处于活动状态时，才会通知此观察者有关包装数据的修改。 如果它的状态是Lifecycle.State.STARTED或Lifecycle.State.RESUMED ，LifecycleOwner 被认为是活动的。 
>
> 通过observeForever(Observer)添加的observeForever(Observer)被视为始终处于活动状态，因此将始终收到有关修改的通知。 对于这些观察者，您应该手动调用removeObserver(Observer) 。
> 如果相应的 Lifecycle 移动到Lifecycle.State.DESTROYED状态，则会自动删除添加了 Lifecycle 的观察者。 这对于活动和片段特别有用，它们可以安全地观察 LiveData 而不必担心泄漏：它们在被销毁时会立即取消订阅。
> 此外，LiveData 有onActive()和onInactive()方法，可以在活动Observer的数量在 0 和 1 之间变化时得到通知。这允许 LiveData 在没有任何 Observer 正在积极观察时释放任何大量资源。
> 此类旨在保存ViewModel各个数据字段，但也可用于以解耦方式在应用程序中的不同模块之间共享数据

由此官方已经给出了LiveData的用途和优点；



#### 

```java
public abstract class LiveData<T> {
    ...
    
    /**
     * 这里是postValue()开启的线程用于主线程设置数据
     */
    private final Runnable mPostValueRunnable = new Runnable() {
        @SuppressWarnings("unchecked")
        @Override
        public void run() {
            Object newValue;
            synchronized (mDataLock) {
                newValue = mPendingData;
                mPendingData = NOT_SET;
            }
            setValue((T) newValue);
        }
    };
    
    public LiveData(T value) {
        mData = value;
        mVersion = START_VERSION + 1;
    }

    public LiveData() {
        mData = NOT_SET;
        mVersion = START_VERSION;
    }

    @SuppressWarnings("unchecked")
    private void considerNotify(ObserverWrapper observer) {
        if (!observer.mActive) {
            return;
        }
        // Check latest state b4 dispatch. Maybe it changed state but we didn't get the event yet.
        //
        // we still first check observer.active to keep it as the entrance for events. So even if
        // the observer moved to an active state, if we've not received that event, we better not
        // notify for a more predictable notification order.
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

    @SuppressWarnings("WeakerAccess") /* synthetic access */
    void dispatchingValue(@Nullable ObserverWrapper initiator) {
        if (mDispatchingValue) {
            mDispatchInvalidated = true;
            return;
        }
        mDispatchingValue = true;
        do {
            mDispatchInvalidated = false;
            if (initiator != null) {
                considerNotify(initiator);
                initiator = null;
            } else {
                for (Iterator<Map.Entry<Observer<? super T>, ObserverWrapper>> iterator =
                        mObservers.iteratorWithAdditions(); iterator.hasNext(); ) {
                    considerNotify(iterator.next().getValue());
                    if (mDispatchInvalidated) {
                        break;
                    }
                }
            }
        } while (mDispatchInvalidated);
        mDispatchingValue = false;
    }

    /**
     * 在给定所有者的生命周期内将给定观察者添加到观察者列表中。 事件在主线程上调度。 如果 LiveData 已经有数据集，它将被传递给观察者。
     * 如果所有者处于Lifecycle.State.STARTED或Lifecycle.State.RESUMED状态（活动），观察者将仅接收事件。
     * 如果所有者移动到Lifecycle.State.DESTROYED状态，观察者将被自动移除。
     * 当owner不活动时数据发生变化时，它将不会收到任何更新。 如果它再次变为活动状态，它将自动接收最后可用的数据。
     * 只要给定的 LifecycleOwner 没有被销毁，LiveData 就会保持对观察者和所有者的强引用。 当它被销毁时，LiveData 删除对观察者和所有者的引用。
     * 如果给定的所有者已经处于Lifecycle.State.DESTROYED状态，LiveData 会忽略调用。
     * 如果给定的所有者、观察者元组已经在列表中，则忽略该调用。 如果观察者已经与另一个所有者在列表中，则 LiveData 会抛出IllegalArgumentException。
     */
    @MainThread
    public void observe(@NonNull LifecycleOwner owner, @NonNull Observer<? super T> observer) {
        assertMainThread("observe");
        if (owner.getLifecycle().getCurrentState() == DESTROYED) {
            // ignore
            return;
        }
        LifecycleBoundObserver wrapper = new LifecycleBoundObserver(owner, observer);
        ObserverWrapper existing = mObservers.putIfAbsent(observer, wrapper);
        if (existing != null && !existing.isAttachedTo(owner)) {
            throw new IllegalArgumentException("Cannot add the same observer"
                    + " with different lifecycles");
        }
        if (existing != null) {
            return;
        }
        owner.getLifecycle().addObserver(wrapper);
    }

    /**
     * Adds the given observer to the observers list. This call is similar to
     * {@link LiveData#observe(LifecycleOwner, Observer)} with a LifecycleOwner, which
     * is always active. This means that the given observer will receive all events and will never
     * be automatically removed. You should manually call {@link #removeObserver(Observer)} to stop
     * observing this LiveData.
     * While LiveData has one of such observers, it will be considered
     * as active.
     * <p>
     * If the observer was already added with an owner to this LiveData, LiveData throws an
     * {@link IllegalArgumentException}.
     *
     * @param observer The observer that will receive the events
     */
    @MainThread
    public void observeForever(@NonNull Observer<? super T> observer) {
        assertMainThread("observeForever");
        AlwaysActiveObserver wrapper = new AlwaysActiveObserver(observer);
        ObserverWrapper existing = mObservers.putIfAbsent(observer, wrapper);
        if (existing instanceof LiveData.LifecycleBoundObserver) {
            throw new IllegalArgumentException("Cannot add the same observer"
                    + " with different lifecycles");
        }
        if (existing != null) {
            return;
        }
        wrapper.activeStateChanged(true);
    }

    /**
     * Removes the given observer from the observers list.
     *
     * @param observer The Observer to receive events.
     */
    @MainThread
    public void removeObserver(@NonNull final Observer<? super T> observer) {
        assertMainThread("removeObserver");
        ObserverWrapper removed = mObservers.remove(observer);
        if (removed == null) {
            return;
        }
        removed.detachObserver();
        removed.activeStateChanged(false);
    }

    /**
     * Removes all observers that are tied to the given {@link LifecycleOwner}.
     *
     * @param owner The {@code LifecycleOwner} scope for the observers to be removed.
     */
    @SuppressWarnings("WeakerAccess")
    @MainThread
    public void removeObservers(@NonNull final LifecycleOwner owner) {
        assertMainThread("removeObservers");
        for (Map.Entry<Observer<? super T>, ObserverWrapper> entry : mObservers) {
            if (entry.getValue().isAttachedTo(owner)) {
                removeObserver(entry.getKey());
            }
        }
    }

    /**
     * Posts a task to a main thread to set the given value. So if you have a following code
     * executed in the main thread:
     * <pre class="prettyprint">
     * liveData.postValue("a");
     * liveData.setValue("b");
     * </pre>
     * The value "b" would be set at first and later the main thread would override it with
     * the value "a".
     * <p>
     * If you called this method multiple times before a main thread executed a posted task, only
     * the last value would be dispatched.
     *
     * @param value The new value
     */
    protected void postValue(T value) {
        boolean postTask;
        synchronized (mDataLock) {
            postTask = mPendingData == NOT_SET;
            mPendingData = value;
        }
        if (!postTask) {
            return;
        }
        ArchTaskExecutor.getInstance().postToMainThread(mPostValueRunnable);
    }

    /**
     * Sets the value. If there are active observers, the value will be dispatched to them.
     * <p>
     * This method must be called from the main thread. If you need set a value from a background
     * thread, you can use {@link #postValue(Object)}
     *
     * @param value The new value
     */
    @MainThread
    protected void setValue(T value) {
        assertMainThread("setValue");
        mVersion++;
        mData = value;
        dispatchingValue(null);
    }

    /**
     * Returns the current value.
     * Note that calling this method on a background thread does not guarantee that the latest
     * value set will be received.
     *
     * @return the current value
     */
    @SuppressWarnings("unchecked")
    @Nullable
    public T getValue() {
        Object data = mData;
        if (data != NOT_SET) {
            return (T) data;
        }
        return null;
    }

    int getVersion() {
        return mVersion;
    }

    /**
     * Called when the number of active observers change from 0 to 1.
     * <p>
     * This callback can be used to know that this LiveData is being used thus should be kept
     * up to date.
     */
    protected void onActive() {

    }

    /**
     * Called when the number of active observers change from 1 to 0.
     * <p>
     * This does not mean that there are no observers left, there may still be observers but their
     * lifecycle states aren't {@link Lifecycle.State#STARTED} or {@link Lifecycle.State#RESUMED}
     * (like an Activity in the back stack).
     * <p>
     * You can check if there are observers via {@link #hasObservers()}.
     */
    protected void onInactive() {

    }

    /**
     * Returns true if this LiveData has observers.
     *
     * @return true if this LiveData has observers
     */
    @SuppressWarnings("WeakerAccess")
    public boolean hasObservers() {
        return mObservers.size() > 0;
    }

    /**
     * Returns true if this LiveData has active observers.
     *
     * @return true if this LiveData has active observers
     */
    @SuppressWarnings("WeakerAccess")
    public boolean hasActiveObservers() {
        return mActiveCount > 0;
    }

    @MainThread
    void changeActiveCounter(int change) {
        int previousActiveCount = mActiveCount;
        mActiveCount += change;
        if (mChangingActiveState) {
            return;
        }
        mChangingActiveState = true;
        try {
            while (previousActiveCount != mActiveCount) {
                boolean needToCallActive = previousActiveCount == 0 && mActiveCount > 0;
                boolean needToCallInactive = previousActiveCount > 0 && mActiveCount == 0;
                previousActiveCount = mActiveCount;
                if (needToCallActive) {
                    onActive();
                } else if (needToCallInactive) {
                    onInactive();
                }
            }
        } finally {
            mChangingActiveState = false;
        }
    }

    class LifecycleBoundObserver extends ObserverWrapper implements LifecycleEventObserver {
        @NonNull
        final LifecycleOwner mOwner;

        LifecycleBoundObserver(@NonNull LifecycleOwner owner, Observer<? super T> observer) {
            super(observer);
            mOwner = owner;
        }

        @Override
        boolean shouldBeActive() {
            return mOwner.getLifecycle().getCurrentState().isAtLeast(STARTED);
        }

        @Override
        public void onStateChanged(@NonNull LifecycleOwner source,
                @NonNull Lifecycle.Event event) {
            Lifecycle.State currentState = mOwner.getLifecycle().getCurrentState();
            if (currentState == DESTROYED) {
                removeObserver(mObserver);
                return;
            }
            Lifecycle.State prevState = null;
            while (prevState != currentState) {
                prevState = currentState;
                activeStateChanged(shouldBeActive());
                currentState = mOwner.getLifecycle().getCurrentState();
            }
        }

        @Override
        boolean isAttachedTo(LifecycleOwner owner) {
            return mOwner == owner;
        }

        @Override
        void detachObserver() {
            mOwner.getLifecycle().removeObserver(this);
        }
    }

    private abstract class ObserverWrapper {
        final Observer<? super T> mObserver;
        boolean mActive;
        int mLastVersion = START_VERSION;

        ObserverWrapper(Observer<? super T> observer) {
            mObserver = observer;
        }

        abstract boolean shouldBeActive();

        boolean isAttachedTo(LifecycleOwner owner) {
            return false;
        }

        void detachObserver() {
        }

        void activeStateChanged(boolean newActive) {
            if (newActive == mActive) {
                return;
            }
            // immediately set active state, so we'd never dispatch anything to inactive
            // owner
            mActive = newActive;
            changeActiveCounter(mActive ? 1 : -1);
            if (mActive) {
                dispatchingValue(this);
            }
        }
    }

    private class AlwaysActiveObserver extends ObserverWrapper {

        AlwaysActiveObserver(Observer<? super T> observer) {
            super(observer);
        }

        @Override
        boolean shouldBeActive() {
            return true;
        }
    }

    static void assertMainThread(String methodName) {
        if (!ArchTaskExecutor.getInstance().isMainThread()) {
            throw new IllegalStateException("Cannot invoke " + methodName + " on a background"
                    + " thread");
        }
    }
}

```



涉及到Lifecycle，LifecycleOwner，LifecycleBoundObserver，ObserverWrapper，LifecycleEventObserver

感知生命周期，是因为observe（）方法传入了LifecycleOwner，ComponentActivity实现了此接口，获取到了Lifecycle

#### **总结**





post是怎样执行的？

开启了一个主线程，由此可以得知设置livedata的值只能在主线程中。











何为粘性事件？
即发射的事件如果早于注册，那么注册之后依然可以接收到的事件称为粘性事件

“数据倒灌”一词出自[KunMinX](https://links.jianshu.com/go?to=https%3A%2F%2Fxiaozhuanlan.com%2Fu%2Fkunminx)的Blog[**重学安卓：LiveData 数据倒灌 背景缘由全貌 独家解析**](https://links.jianshu.com/go?to=https%3A%2F%2Fxiaozhuanlan.com%2Ftopic%2F6719328450),即在setValue后,observe对此次set的value值会进行多次消费。比如进行第二次observe的时候获取到的数据是第一次的旧数据。这样会带来不可预期的后果。



https://www.jianshu.com/p/e08287ec62cd

https://mp.weixin.qq.com/s/o61NDIptP94X4HspKwiR2w

https://www.jianshu.com/p/d0244c4c7cc9
