---
layout: post
title:  "Metabaseのお勉強"
date:   2018-12-03 10:00:06 +0900
categories: clojure metabase bi
---
Clojureのキラーアプリ候補として[Metabase][metabase]があったので
実験しています。
[ここ][metabase download]からmetabase.jarをダウンロードします。
コマンドラインから
~~~
java -jar metabase.jar
~~~
で動かします。
Browserから[ここ][metabase runtime]でアクセスできます。
あら簡単！

ただデフォルトがh2のデータベースなんですよね。
流石にこれではバックアップやら何やらで辛いので
Postgresqlに移行します。

[移行ページ][metabase migration]にあります。

事前にPostgreSQLにmetabaseでCREATE DATABASEしておきます。

マイグレイションShell
~~~
export MB_DB_TYPE=postgres
export MB_DB_DBNAME=metabase
export MB_DB_PORT=5432
export MB_DB_USER=<username>
export MB_DB_PASS=<password>
export MB_DB_HOST=localhost
java -jar metabase.jar load-from-h2 /path/to/metabase.db #  .mv.db や .h2.db なサフィックスは不要です。ディレクトリ/metabase.dbでOK
~~~

Metabase起動shell
~~~
export MB_DB_TYPE=postgres
export MB_DB_DBNAME=metabase
export MB_DB_PORT=5432
export MB_DB_USER=<username>
export MB_DB_PASS=<password>
export MB_DB_HOST=localhost
java -jar metabase.jar 
~~~

これで簡単に動きました。こりゃええわぁ

[metabase runtime]:http://localhost:3000/
[metabase]:https://www.metabase.com/
[metabase download]:https://www.metabase.com/start/jar.html
[metabase migration]:https://www.metabase.com/docs/v0.29.2/operations-guide/start.html#configuring-the-metabase-application-database
