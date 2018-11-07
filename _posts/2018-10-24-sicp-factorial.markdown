---
layout: post
title:  "SICP FactorialをClojureで実装してみた"
date:   2018-10-24 08:31:06 +0900
categories: clojure sicp
---
またまた階乗です。ヲレはこの手のコードはデフォルトiterate使って
遅延シーケンスつくるです。

{% highlight clojure %}
(take 10 (iterate (fn[[a b]][(* a b) (inc b)]) [1 2]))
{% endhighlight %}

Clojureだと末尾最適化が？なのではありますがコーンなコードでいかがでしょうか。

{% highlight clojure %}
(defn fact-iter [product counter max-count]
  (if (> counter max-count)
    product
    (recur (* counter product)
           (inc counter )
           max-count))
  )
(defn factorial [n]
  (fact-iter 1 1 n))
(factorial 6)
{% endhighlight %}
