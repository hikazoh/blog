---
layout: post
title:  "Apache MXNETをClojureから使う。Docker版"
date:   2018-11-07 09:35:00 +0900
categories: clojure mxnet docker
---
お断り(disclaimer):もちろんこの手順は無保証です。
以前すっげー頑張って自分の環境でつくったのではありますが、
あとでDockerが[magnetcoop/mxnet-clj-cpu docker]あることに気づいてこっちに乗り換えました。
説明[magnet-coop/mxnet-clj-cpu]もあって楽です。Dockerはいいですなぁ。

{% highlight shell %}
sudo docker pull magnetcoop/mxnet-clj-cpu
{% endhighlight %}

[magnet-coop/mxnet-clj-cpu]: https://bitbucket.org/magnet-coop/mxnet-clj-cpu
[magnetcoop/mxnet-clj-cpu docker]: https://bitbucket.org/magnet-coop/mxnet-clj-cpu

