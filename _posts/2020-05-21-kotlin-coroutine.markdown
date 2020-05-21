---
layout: post
title:  "ClojureとApache MXNETのndarrayでゼロから作るDeepLearningを実装中"
date:   2019-05-29 09:00:00 +0900
categories: android kotlin
---
お断り(disclaimer):もちろんこの手順は無保証です。
[Androidな本][AndroidBook]と[Kotlinな本][KotliBook]で勉強してました
KotlinでもCoroutineが使えるってので非同期で動かして画面への反映を簡単にできるかなぁ
ってので試してみました。
Activityの要素をmain thread以外から書き換えるとruntime errorになっちゃうのでそこはViewModel(LiveData)
でObserverパタンを使いました
Codeは[ここ][github]です
## build.gradleでcoroutineをダンロード

appのbuild.gradleで2行追加
~~~
dependencies {
   implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.3.7'
   implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.3.7'
}
~~~

## Layoutを定義する

[ここ][layout]を参考にしてください。
![スクリーン](/blog/images/screen.PNG)でTextViewとButtonの簡単な処理です

##ソースを書く
[ここ][src]にコードを置きました。

こいつは５秒まってから現在時刻を取得するObjectです
{% highlight kotlin %}

{GiantJobs {
    suspend fun  getNowAfterGiantJob(ctx: CoroutineScope) :Deferred<String>{
        return ctx.async {
            Thread.sleep(5000L)
            getNow()
        }
    }

    fun getNow() : String = SimpleDateFormat("yyyy-MM-dd hh:mm:ss").format(Calendar.getInstance().time)

{% endhighlight %}

ViewModelのなかで値が変わったらPropertyに反映させます。
{% highlight kotlin %}
class StringViewModel : ViewModel(){
    private  val str : MutableLiveData<String> by lazy {
            MutableLiveData<String>()
    }
    fun getStr() : LiveData<String> {
        return str
    }

    fun updateData() {
        GlobalScope.launch {
            str.postValue(GiantJobs.getNowAfterGiantJob(this).await())
        }
    }
}
{% endhighlight %}

onCreateでViewModelを配置して値のObserveとButtonのClick処理を入れ込みました

{% highlight kotlin %}
        var button = findViewById<Button>(R.id.button)
        val model = ViewModelProvider.NewInstanceFactory().create(StringViewModel::class.java)
        model.getStr().observe(this, androidx.lifecycle.Observer {
            dateTextView.text = it
        })
        button.setOnClickListener {
            model.updateData()
        }
{% endhighlight %}

これでButton押下後の５秒後の時刻を表示します。

[AndroidBook]: https://amzn.to/3e3MlCl
[KotlinBook]: https://amzn.to/2LM5mxc
[github]: https://github.com/hikazoh/KotlinCoroutine
[layout]: https://github.com/hikazoh/KotlinCoroutine/blob/master/app/src/main/res/layout/activity_main.xml
[src]: https://github.com/hikazoh/KotlinCoroutine/blog/master/app/src/main/java/com/example/coroutine/MainActivity.kt
