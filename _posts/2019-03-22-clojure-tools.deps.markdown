---
layout: post
title:  "Clojure CLIでUberjarやTestやGraalVMをやってみた"
date:   2019-03-22 09:00:00 +0900
categories: clojure graalvm tools-deps
---
お断り(disclaimer):もちろんこの手順は無保証です。

[前回][before][GraalVM][graalvm]を使ってLinuxの実行モジュールを作りました。
色々と調べると[Cambada][cambada]があってこっちのほうがなんとなーく簡単な
感じだったのでお試ししました。
環境変数はこのままで変更してません。少なくともPathを通しておく必要は
あるですね。

~~~
export GRAALVM_HOME=${INSTALLED_DIR}
export PATH=${PATH}:${GRAALVM_HOME}/bin
~~~

treeした結果はこーんな感じです。

~~~
.
├── deps.edn
├── src
│   └── main
│       └── core.clj
└── test
    └── main
        └── core_test.clj

4 directories, 3 files


~~~
Clojureのソースは

{% highlight clojure %}
(ns main.core
  (:gen-class))

(defn -main[]
  (print "Hello,World"))

{% endhighlight %}

です。大したことやっていません。

TestCodeは
{% highlight clojure %}
(ns main.core-test
  (:require [clojure.test :as t]
            [main.core :as sut]))

(t/deftest basic-tests
  (t/testing "it says hello to everyhone"
    (t/is (= (with-out-str (sut/-main)) "Hello,World"))))
{% endhighlight %}

deps.ednはコーンな感じ
~~~
{:deps
   {org.clojure/clojure {:mvn/version "1.10.0"}}
 :aliases
   {:test
     {:extra-paths ["test"]
      :extra-deps 
        {com.cognitect/test-runner 
          {:git/url "https://github.com/cognitect-labs/test-runner.git"
           :sha "209b64504cb3bd3b99ecfec7937b358a879f55c1"}}
      :main-opts ["-m" "cognitect.test-runner"]}
    :uberjar
      {:extra-deps
        {luchiniatwork/cambada
          {:mvn/version "1.0.0"}}
       :main-opts  ["-m" "cambada.uberjar" "-m"  "main.core"] }
    :native-image
      {:extra-deps
        {luchiniatwork/cambada
          {:mvn/version "1.0.0"}}
       :main-opts  ["-m" "cambada.native-image" "-m"  "main.core"] }
} }

~~~
Testは[Cognitect Test Runner][testrunner]を使いました。
あとはREADME.MDのままです。

これでCLIから
~~~
clj -A:test
~~~
ってやるとTest Runnerが動いてくれます。
~~~
Running tests in #{"test"}

Testing main.core-test

Ran 1 tests containing 1 assertions.
0 failures, 0 errors.
~~~

ではCLIから
~~~
clj -A:uberjar
~~~
で
~~~
Cleaning target
Creating target/classes
  Compiling main.core
Creating target/main-1.0.0-SNAPSHOT.jar
Updating pom.xml
Creating target/main-1.0.0-SNAPSHOT-standalone.jar
  Including main-1.0.0-SNAPSHOT.jar
  Including clojure-1.10.0.jar
  Including spec.alpha-0.2.176.jar
  Including core.specs.alpha-0.2.44.jar
Done!
~~~
でtarget配下にjarができちゃいます。

またCLIから
~~~
clj -A:native-image
~~~
ってやると
~~~
Cleaning target
Creating target/classes
  Compiling main.core
Creating target/main
[target/main:5489]    classlist:   5,314.19 ms
[target/main:5489]        (cap):   2,082.70 ms
[target/main:5489]        setup:   5,900.93 ms
[target/main:5489]   (typeflow):  18,074.05 ms
[target/main:5489]    (objects):   3,867.74 ms
[target/main:5489]   (features):     263.63 ms
[target/main:5489]     analysis:  22,537.37 ms
[target/main:5489]     universe:     694.03 ms
[target/main:5489]      (parse):   6,022.45 ms
[target/main:5489]     (inline):   5,749.60 ms
[target/main:5489]    (compile):  35,960.57 ms
[target/main:5489]      compile:  48,706.00 ms
[target/main:5489]        image:   2,009.25 ms
[target/main:5489]        write:   1,523.79 ms
[target/main:5489]      [total]:  86,855.55 ms

Done!
~~~

targetの配下に実行モジュールができます。

これをベースにして色々と実験してみるかのぉ。

[cambada]: https://github.com/luchiniatwork/cambada
[before]: https://hikazoh.github.io/blog/clojure/graalvm/2019/03/19/graalvm-clojure.html
[graalvm]: https://www.graalvm.org/
[testrunner]: https://github.com/cognitect-labs/test-runner
