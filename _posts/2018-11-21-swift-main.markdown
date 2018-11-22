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

それとねOptionalがSwiftでは素晴らしいそうなのでちょっと実験

参考：[Swift Study][github]でいれました。


{% highlight swift %}
import Foundation

func mySqrt(_ x: Double) -> Double? {
	return x < 0.0 ? nil : sqrt(x)
}

class Person {
    let classProp = "Person"
    var name:String = ""
    var id:Int32 = 0
}

print("TEST")

if let d = mySqrt(10.0) {
    print(d)
}else{
    print("Nil No1")
}

if let d = mySqrt(-10.9){
    print(d)
}else{
    print("Nil No2")
}

var c:Person?

c = Person()
c!.name = "abc"
if let d = c {
    print(d.name)
}else{
    print("Nil No3")
}

c = nil
print(c?.name as Any)
if let d = c {
    print(d.name)
}else{
    print("Nil No4")
}

exit(0)

{% endhighlight %}

実行結果
    TEST
    3.1622776601683795
    Nil No2
    abc
    nil
    Nil No4

[github]: https://github.com/hikazoh/swift_study

