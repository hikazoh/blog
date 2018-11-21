---
layout: post
title:  "Swiftのお勉強"
date:   2018-11-21 19:00:06 +0900
categories: swift
---
Clojureの合間とかでSwiftも勉強しています。
会社ではLinuxなので文法だけです。

エントリーポイントってCなどになれていると
main関数を定義しちゃいたくなります。
Swiftで簡単なのはmain.swiftってファイルをつくっちゃえば
そこの記載がエントリーポイントになっちゃうようです。

参考：[Swift Study][github]でいれました。


{% highlight swift %}
import Foundation

func mySqrt(_ x: Double) -> Double? {
	return x < 0.0 ? nil : sqrt(x)
}

print("TEST")

if let d = mySqrt(10.0) {
    print(d)
}else{
    print("Nil")
}

if let d = mySqrt(-10.9){
    print(d)
}else{
    print("Nil")
}


exit(0)

{% endhighlight %}

[github]: https://github.com/hikazoh/swift_study

