# Retrofit

## 一、简单使用

### 1.1 创建客户端对象

```kotlin
private val retrofit: Retrofit by lazy {
    // 创建okhttp客户端进行参数配置
    // retrofit没有任何网络请求的功能
    val client = OkHttpClient.Builder().apply {
        // 进行各项参数的配置
        // 详细请参照OkHttp
    }.build()
    
    // 这才是真正的retrofit客户端
    Retrofit.Builder()
    	.baseUrl(url) 	// url必须以‘/’结尾
    	.client(client)
    	.build()
}
```

总结：

* 第一步创建OkHttpClient对象，当然这不是必须的步骤<b>(因为Retrofit对象中有默认的OkHttpClient对象实现)</b>，这个对象往往无法满足我们复杂的需求场景，所以大部分情况下我们都需要自定义这个client。

* 创建Retrofit对象，传入url。



### 1.2 创建服务提供者

```kotlin
interface Services {
    @GET("get")
    suspend fun getData(): Call<ResponseBody>
}
```

总结：

* 服务提供者中封装了我们在网络协议中的大部分数据，之后会详细分析
* 由于Android中网络请求必须在工作线程进行并且属于耗时操作，因此要标记为挂起函数，方便协程调用
* 