---
layout: post
title:  "ClojureとApache MXNETのndarrayでゼロから作るDeepLearningを実装中"
date:   2019-05-29 09:00:00 +0900
categories: clojure mxnet deeplearning
---
お断り(disclaimer):もちろんこの手順は無保証です。
[ゼロから作るDeepLearning][zero]でDeepLearningを勉強してます。
[カマイルカ][ref]さんが[core.matrix][matrix]で実装なさっているので、
当方は[mxnet clojure][mxnetclojure]での実装を試みました。
そもそもApache MXNET自体がDeepLearningの実装なのですが、その使い方を
学ぶ上で車輪の再発明でもいいかぁって思った次第です。
まだ２章のAND・OR・XORで力尽きたのであとで書きます。
かーんたんなソースはここに載せました。
[MXNET Clojure][github]です。
これ全部実装するのはしんどいので今後はClojureでMXNETを使ったDeepLearningに
関して何か記すことができればと思いながら仕事に追われています。

[zero]: http://amzn.to/2i2d4Up
[ref]: https://scrapbox.io/lagenorhynque/Clojure%E3%81%A7%E3%80%8E%E3%82%BC%E3%83%AD%E3%81%8B%E3%82%89%E4%BD%9C%E3%82%8BDeep_Learning%E3%80%8F%E5%AD%A6%E7%BF%92%E3%83%A1%E3%83%A2
[matrix]: https://github.com/mikera/core.matrix
[mxnetclojure]: https://mxnet.incubator.apache.org/api/clojure/index.html
[github]: https://github.com/hikazoh/ZeroDeepLearning
