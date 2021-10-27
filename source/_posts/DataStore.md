---
title: DataStore
date: 2021-10-26 14:27:23
tags: Kotlin
categories: Kotlin

---

> DataStore 提供了一种安全且持久的方式来存储少量数据，例如首选项和应用程序状态。 它不支持部分更新：如果任何字段被修改，整个对象将被序列化并持久化到磁盘。 如果您想要部分更新，请考虑使用 Room API (SQLite)。
> DataStore 提供 ACID 保证。 它是线程安全的，并且是非阻塞的。 特别是，它解决了 SharedPreferences API 的这些设计缺陷：
>
> - 同步 API 鼓励 StrictMode 违规
> - apply() 和 commit() 没有发出错误信号的机制
> - apply() 将阻塞 fsync() 上的 UI 线程
> - 不持久——它可以返回尚未持久化的状态
> - 没有一致性或事务语义
> - 解析错误时引发运行时异常
> - 公开对其内部状态的可变引用





记录出现的问题：
登录页保存登录名和密码；获取时转成了livedata，并观察其数据变化，当点击了登录，会打印两遍！

```kotlin
val loginName: LiveData<String>? =
    DataStorePreferences(MyApplication.myApplication).getStringFlowValue(
        DataStorePreferences.LOGIN_NAME
    )?.asLiveData()
val loginPassword: LiveData<String>? =
    DataStorePreferences(MyApplication.myApplication).getStringFlowValue(
        DataStorePreferences.LOGIN_PASSWORD
    )?.asLiveData()
...
//点击登录，保存数据
fun saveUserInfo(username: String, password: String) {
        viewModelScope.launch {
            DataStorePreferences(MyApplication.myApplication).setStringValue(
                key = DataStorePreferences.LOGIN_NAME, value = username
            )
            DataStorePreferences(MyApplication.myApplication).setStringValue(
                key = DataStorePreferences.LOGIN_PASSWORD,
                value = password
            )
        }
    }
...
loginViewModel.loginPassword?.observe(this@LoginActivity, Observer {
            val loginPassword = it ?: return@Observer
            Log.e(TAG, "loginPassword: ${loginPassword}")
        })
```

正如DataStore的类注释：`如果任何字段被修改，整个对象将被序列化并持久化到磁盘`，因此对于livedata来说，保存了一遍用户名，保存了一遍密码，认为数据有两次变化，所以会打印两遍；

修改代码如下：

```kotlin
//保存的时候，一起保存
suspend fun setNameAndPassWord(@NotNull name: String, passWord: String) {
    context?.applicationContext?.dataStore?.edit { settings ->
        settings.putAll(
            stringPreferencesKey(LOGIN_NAME) to name,
            stringPreferencesKey(LOGIN_PASSWORD) to passWord
        )
    }
}
```

