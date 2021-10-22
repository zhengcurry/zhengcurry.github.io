title: ViewModel如何数据共享
date: 2021-10-22 13:51:37
tags: Android
categories: Android

------

当我们创建一个ViewModel类，获取其实例是通过ViewModelProvider
```
val testViewModel = ViewModelProvider(this).get(TestViewModel::class.java)
```
或者采用kotlin中的viewModels
```
private val testViewModel by viewModels<TestViewModel>()
```

activity和fragment组件之间的数据共享，这个不用多说，因为ViewModel是activity销毁的时候才clear，作用域是activity的生命周期，因此fragment可以持有同一个ViewModel的示例，从而共享activity的数据



ViewModelProvider(this) 的参数是ViewModelStoreOwner接口，ComponentActivity实现了ViewModelStoreOwner；由此把ViewModel和Activity关联起来



ViewModel的作用：

```
ViewModel 是一个负责为Activity或Fragment准备和管理数据的类。 它还处理 Activity/Fragment 与应用程序其余部分的通信（例如调用业务逻辑类）。
ViewModel 始终与作用域（片段或活动）相关联地创建，并且只要作用域处于活动状态就会保留。 例如，如果它是一个活动，直到它完成。
换句话说，这意味着如果 ViewModel 的所有者因配置更改（例如旋转）而被销毁，则它不会被销毁。 新的所有者实例只是重新连接到现有模型。
ViewModel 的目的是获取和保存 Activity 或 Fragment 所需的信息。 Activity 或 Fragment 应该能够观察到 ViewModel 中的变化。 ViewModel 通常通过LiveData或 Android 数据绑定公开这些信息。 您还可以使用您喜欢的框架中的任何可观察性构造。
ViewModel 的唯一职责是管理 UI 的数据。 它永远不应该访问您的视图层次结构或保留对 Activity 或 Fragment 的引用。
```

- 1.规范化了`ViewModel`的基类；
- 2.`ViewModel`不会随着`Activity`的屏幕旋转而销毁；
- 3.在对应的作用域内，保正只生产出对应的唯一实例，保证UI组件间的通信





**如何实现屏幕旋转数据也会保存的？**

```java
//----ComponentActivity	
	/**
     * Retain all appropriate non-config state.  You can NOT
     * override this yourself!  Use a {@link androidx.lifecycle.ViewModel} if you want to
     * retain your own non config state.
     * 保留所有适当的非配置状态。 你不能自己覆盖它！ 
     * 如果您想保留自己的非配置状态，请使用androidx.lifecycle.ViewModel 
     */
    @Override
    @Nullable
	//保留非配置实例
    public final Object onRetainNonConfigurationInstance() {
        Object custom = onRetainCustomNonConfigurationInstance();

        ViewModelStore viewModelStore = mViewModelStore;
        if (viewModelStore == null) {
            // No one called getViewModelStore(), so see if there was an existing
            // ViewModelStore from our last NonConfigurationInstance
            NonConfigurationInstances nc =
                    (NonConfigurationInstances) getLastNonConfigurationInstance();
            if (nc != null) {
                viewModelStore = nc.viewModelStore;
            }
        }

        if (viewModelStore == null && custom == null) {
            return null;
        }

        NonConfigurationInstances nci = new NonConfigurationInstances();
        nci.custom = custom;
        nci.viewModelStore = viewModelStore;
        return nci;
    }

	/**
     * Returns the {@link ViewModelStore} associated with this activity
     * <p>
     * 返回与此activity关联的ViewModelStore
     */
    @NonNull
    @Override
    public ViewModelStore getViewModelStore() {
        ...
        if (mViewModelStore == null) {
            NonConfigurationInstances nc =
                    (NonConfigurationInstances) getLastNonConfigurationInstance();
            if (nc != null) {
                // Restore the ViewModelStore from NonConfigurationInstances
                mViewModelStore = nc.viewModelStore;
            }
            if (mViewModelStore == null) {
                mViewModelStore = new ViewModelStore();
            }
        }
        return mViewModelStore;
    }
```

根据上面ComponentActivity中的源码，可以看到当调用getViewModelStore（）时，会先去获取之前的实例getLastNonConfigurationInstance()，也是就是我们ViewModel中的数据，而方法onRetainNonConfigurationInstance（）是覆写的，其触发方式，父类Activity中的方法注释也指出`Called by the system`







```java
/**
     * Returns an existing ViewModel or creates a new one in the scope (usually, a fragment or
     * an activity), associated with this {@code ViewModelProvider}.
     * <p>
     * The created ViewModel is associated with the given scope and will be retained
     * as long as the scope is alive (e.g. if it is an activity, until it is
     * finished or process is killed).
     */
    @NonNull
    @MainThread
    public <T extends ViewModel> T get(@NonNull Class<T> modelClass) {
        String canonicalName = modelClass.getCanonicalName();
        if (canonicalName == null) {
            throw new IllegalArgumentException("Local and anonymous classes can not be ViewModels");
        }
        return get(DEFAULT_KEY + ":" + canonicalName, modelClass);
    }

    @SuppressWarnings("unchecked")
    @NonNull
    @MainThread
    public <T extends ViewModel> T get(@NonNull String key, @NonNull Class<T> modelClass) {
        ViewModel viewModel = mViewModelStore.get(key);

        if (modelClass.isInstance(viewModel)) {
            if (mFactory instanceof OnRequeryFactory) {
                ((OnRequeryFactory) mFactory).onRequery(viewModel);
            }
            return (T) viewModel;
        } else {
            //noinspection StatementWithEmptyBody
            if (viewModel != null) {
                // TODO: log a warning.
            }
        }
        if (mFactory instanceof KeyedFactory) {
            viewModel = ((KeyedFactory) mFactory).create(key, modelClass);
        } else {
            viewModel = mFactory.create(modelClass);
        }
        mViewModelStore.put(key, viewModel);
        return (T) viewModel;
    }
```



ViewModel和ViewModelStore的源码都很简单



ViewModelStore内部就是HashMap存储ViewModel

```java
public class ViewModelStore {

    private final HashMap<String, ViewModel> mMap = new HashMap<>();

    final void put(String key, ViewModel viewModel) {
        ViewModel oldViewModel = mMap.put(key, viewModel);
        if (oldViewModel != null) {
            oldViewModel.onCleared();
        }
    }

    final ViewModel get(String key) {
        return mMap.get(key);
    }

    Set<String> keys() {
        return new HashSet<>(mMap.keySet());
    }

    /**
     *  Clears internal storage and notifies ViewModels that they are no longer used.
     */
    public final void clear() {
        for (ViewModel vm : mMap.values()) {
            vm.clear();
        }
        mMap.clear();
    }
}
```

