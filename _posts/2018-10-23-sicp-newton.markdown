---
layout: post
title:  "SICP Newton"
date:   2018-10-23 09:31:06 +0900
categories: clojure
---
忙中閑ありな感じでSICPをまたまた読んでいます。
Clojureでコード書いてみます。

まずはニュートン法による平方根です。

{% highlight clojure %}
(defn sqrt-itr[guess x]
  (lazy-seq
   (cons guess
         (sqrt-itr (/ (+ guess (/ x guess)) 2) x))))
{% endhighlight %}

