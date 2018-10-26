---
layout: post
title:  "SICP Small Change"
date:   2018-10-26 12:31:06 +0900
categories: clojure sicp
---
1$を1¢ 5¢ 10¢ 25¢ 50¢で何種類両替できるかです。
これも再帰でできちゃうんですよね。このあたりから？？的な度合いが高くなる。


まずは写経から。

{% highlight clojure %}
(declare cc)
(declare first-denomination)
(defn count-change[amount]
  (cc amount 5))
(defn cc[amount kinds-of-coins]
  (cond
    (zero? amount ) 1
    (or (< amount 0) (zero? kinds-of-coins)) 0
     :else
     (+ (cc amount (dec kinds-of-coins))
        (cc (- amount
               (first-denomination
                kinds-of-coins))
            kinds-of-coins))))
(defn first-denomination [kinds-of-coins]
  (cond (= kinds-of-coins 1) 1
        (= kinds-of-coins 2) 5
        (= kinds-of-coins 3) 10
        (= kinds-of-coins 4) 25
        (= kinds-of-coins 5) 50))
(count-change 100)
{% endhighlight %}
