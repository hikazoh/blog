---
layout: post
title:  "Apache MXNETをClojureで使える環境を作る"
date:   2018-11-07 10:35:00 +0900
categories: clojure mxnet docker
tags: clojure mxnet docker
---
お断り(disclaimer):もちろんこの手順は無保証です。
参考というかこのblog[mxnet clojure]をなぞります。

まずはincubator-mxnetをcloneします。
{% highlight git %}
git clone https://github.com/apache/incubator-mxnet
{% endhighlight %}

そのあとでlibmxnet.aをつくります。
{% highlight bash %}
make
{% endhighlight %}

jni.hがないなどといって怒られました。 jdkのインストールディレクトリを${HOME}/bin/jdkとしておきます。そしてMakefileを下記のように修正しました。

{% highlight diff %}
--- a/Makefile
+++ b/Makefile
@@ -76,7 +76,7 @@ include $(DMLC_CORE)/make/dmlc.mk
 
 # all tge possible warning tread
 WARNFLAGS= -Wall -Wsign-compare
-CFLAGS = -DMSHADOW_FORCE_STREAM $(WARNFLAGS)
+CFLAGS = -DMSHADOW_FORCE_STREAM $(WARNFLAGS) -I${HOME}/bin/jdk/include -I${HOME}/bin/jdk/include/linux
 
 ifeq ($(DEV), 1)
        CFLAGS += -g -Werror

{% endhighlight %}

ここで再度
{% highlight bash %}
make
{% endhighlight %}

で無事

{% highlight bash %}
lib/libmxnet.a lib/libmxnet.so
{% endhighlight %}

ができました。

そのあとでscala環境でjarを作ります。これがはまった。

まずはなーんに考えず

{% highlight bash %}
 make scalapkg
{% endhighlight %}

やるとエラー吐くですよねぇ。 そこでエラーメッセージを調べてみるとどーもscalaとjdk10の相性の問題っぽいっことのように気が付きました 面倒なのでjdk10→jdk8へ変えて再度コンパイルしました。 無事成功したのでインストールもしておきます。 結果、↓のjarを採用します。

{% highlight java %}
${INSTALLED_DIRECTORY}scala-package/assembly/linux-x86_64-cpu/target/mxnet-full_2.11-linux-x86_64-cpu-1.3.0-SNAPSHOT.jar
{% endhighlight %}

この時点で漸くclojureのlibraryをcompileできます。

{% highlight clojure %}
 lein uberjar
{% endhighlight %}

これで漸くサンプルを動かすことができます。

imclassificationを動かすためにエラーを一つづつ解消してproject.cljを↓で動いたように見えます。

{% highlight clojure %}
(defproject imclassification "0.1.0-SNAPSHOT"
  :description "Clojure examples for image classification"
  :plugins [[lein-cljfmt "0.5.7"]]
  :dependencies [[org.clojure/clojure "1.9.0"] 
                 [riddley "0.1.15"]
                 [clj-tuple "0.2.2"]
                 [org.clojure/tools.logging "0.4.0"]
                 [org.apache.logging.log4j/log4j-core "2.8.1"]
                 [org.apache.logging.log4j/log4j-api "2.8.1"]
                 [org.slf4j/slf4j-log4j12 "1.7.25" :exclusins [org.slf4j/slf4j-api]]]
  :resource-paths ["${HOME}/clojure/incubator-mxnet/contrib/clojure-package/target/clojure-mxnet-1.3.0-SNAPSHOT.jar" 
                   "${HOME}/clojure/incubator-mxnet/scala-package/assembly/linux-x86_64-cpu/target/mxnet-full_2.11-linux-x86_64-cpu-1.3.0-SNAPSHOT.jar"]
  :pedantic? :skip
  :main imclassification.train-mnist)
{% endhighlight %}

[mxnet clojure]: https://github.com/apache/incubator-mxnet/tree/master/contrib/clojure-package#cloning-the-repo-and-running-from-source
