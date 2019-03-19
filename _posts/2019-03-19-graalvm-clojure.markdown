---
layout: post
title:  "Clojure CLIでGraalVMなモジュールをつくる"
date:   2019-03-19 09:00:00 +0900
categories: clojure graalvm
---
お断り(disclaimer):もちろんこの手順は無保証です。

Clojureの起動の遅さをどーすっかなぁって思って調べると、[GraalVM][graalvm]ってのがあって気になっておりました。
あれこれ調べてみると[Build GraalVM native images with Clojure Deps and CLI tools ][nativeimage]ってのが楽そうなので試してみました。
ふつーにREADMEに従ってやってみたのですが、GraalVMのbinaryにPATHを通した
だけではだめっぽくって環境変数も設定しました。

~~~
export GRAALVM_HOME=${INSTALLED_DIR}
export PATH=${PATH}:${GRAALVM_HOME}/bin
~~~
Clojure CLIつかうので構成は下記の通り。classesがあるのはご愛嬌です。
gitの使い方がまだ下手なだけです。

~~~
.
├── README.md
├── classes
│   ├── core$_main.class
│   ├── core$fn__852.class
│   ├── core$loading__6706__auto____850.class
│   ├── core.class
│   └── core__init.class
├── deps.edn
└── src
    └── core.clj

2 directories, 8 files

~~~
Clojureのソースは

{% highlight clojure %}
(ns core
  (:gen-class))
(defn -main[& args]
(println "Hello, World!"))
{% endhighlight %}

です。大したことやっていません。

deps.ednはコーンな感じ
~~~
{:deps {org.clojure/clojure {:mvn/version "1.10.0"}}
 :aliases {:native-image
           {:main-opts ["-m clj.native-image core"]
            :extra-deps
            {clj.native-image
             {:git/url "https://github.com/taylorwood/clj.native-image.git"
              :sha "498baa963e914fd817dbf33ea251729efd0c8f95"}}}}
  :jvm-opts ["-Dclojure.compiler.direct-linkng-true"]
}
~~~

これでCLIから
~~~
clj -A:native-image
~~~
でうちこむと実行モジュールができちゃいます。
こりゃええわぁ。

[graalvm]: https://www.graalvm.org/
[nativeimage]: https://github.com/taylorwood/clj.native-image
