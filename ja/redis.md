\thispagestyle{empty}
\changepage{}{}{}{-0.5cm}{}{2cm}{}{}{}
![The Little Redis Book, By Karl Seguin](title.png)\

\clearpage
\changepage{}{}{}{0.5cm}{}{-2cm}{}{}{}

## この本について

### ライセンス

The Little Redis Book は the Attribution-NonCommercial 3.0 Unported license の元でライセンスされます｡この本にお金を払う必要はなく､またそうすべきではありません｡

複製､配布､修正､提示は自由に行えますが､この本の著者が私 Karl Seguin であることを常に明らかにしてくれるようお願いします｡また､この本を商用利用してはいけません｡

**ライセンス**の*全文*は以下で読むことが出来ます｡

<http://creativecommons.org/licenses/by-nc/3.0/legalcode>

### 著者について

Karl Seguin は様々な分野と技術に長けた開発者です｡オープンソースプロジェクトの活発な貢献者であり､技術記事の著者であり､時に講演者でもあります｡彼はRedisの他にもいくつかのツールについて様々な記事を書いています｡
Redisは彼のカジュアルゲーム開発者向けの3つのサービス: [mogade.com](http://mogade.com/)でランキングや統計にその力を発揮しています｡

Karl は [The Little MongoDB Book](http://openmymind.net/2011/3/28/The-Little-MongoDB-Book/) という本も書きました｡この本はMongoDBについて書かれており､フリーで人気があります｡

彼のブログは <http://openmymind.net> で､ Twitterアカウントは [@karlseguin](http://twitter.com/karlseguin)
 です

### 謝辞

目と心と情熱で私を助けてくれた [Perry Neal](https://twitter.com/perryneal) に特別な感謝を捧げます｡貴方の助けはかけがえなのないものでした､ありがとう｡

### 最新版

この本の最新版は以下から入手できます｡
<http://github.com/karlseguin/the-little-redis-book>

\clearpage

## はじめに

過去何年かに渡って､データ永続化と問い合わせの技術やテクニックは信じがたいペースで成長してきました｡
RDBではどうにもならないところに来ているというのは確実なところですが､一方でデータを取り巻くエコシステムはその限界を超えていくだろうとも言えるでしょう｡(訳者注：訳文意味不明瞭､原文を以下に残す)
While it's safe to say that relational databases aren't going anywhere, we can also say that the ecosystem around data is never going to be the same.

あらゆる新しいツールやソリューションの中で､Redisは私にとってもっともエキサイティングです｡その理由の第一は､信じがたいほどに簡単に学べるということです｡Redisに慣れるのにかかる時間といえば数時間といったところでしょう｡第二に､Redisは特定の問題を解決すると同時にかなり一般的であるということです｡つまりどういうことかというと､Redisはあらゆるデータに対して何でもかんでもやろうとしません｡Redisを知るにつれ､何がRedisに適していて､何がそうでないか､徐々にはっきりしていくでしょう｡そして全てが明らかになった時､開発者にとってそれは素晴らしい体験です｡

Redisだけを使って全てのシステムを作ることは出来ますが､ほとんどの人はRedisを伝統的なRDBやドキュメント指向データベースといった一般的なデータソリューションの補助と見るだろうと思います｡
Redisは特定の機能を実現するための手段です｡その意味で､インデックスエンジンに似ています｡アプリケーションをLuceneだけで作ったりはしないでしょう｡しかし強力な検索が必要なら､ユーザにとっても開発者にとってもより良い選択です｡もちろんRedisとインデックスエンジンで似ているところといえばそれだけですが｡

この本の目的はRedisを使いこなすための基礎を学ぶことです｡Redisの5つのデータ構造に焦点を当てて､様々なデータモデリングアプローチを検討します｡また同時にいくつかの主な管理とデバッグの方法にも触れます｡

## さあ､はじめよう

我々は皆異なった学び方をします｡ある人は手を動かすことを好み､またある人はビデオを見ることを好み､さらにある人は読むことを好みます｡
Redisを理解するのには実際に触ってみるのが一番です｡Redisのインストールは簡単で､必要なことは全部できる対話環境が使えます｡ここで少し時間をとって自分のマシンでRedisを動かしてみましょう｡

### Windows環境

Redisは公式にはWindowsをサポートしていません｡しかしいくつか方法があります｡これらのやり方を実運用環境で使いたいとは思わないでしょうが､個人的には開発環境で制限に出くわしたことはありません｡

ます､ここ <https://github.com/dmajkic/redis/downloads> にいって､最新のバージョンをダウンロードしてください
(リストの一番上にあるのがそうです)

zipファイルを解凍し､32bitか64bitのうち適切な方のフォルダを開きます｡

### *nix環境とMacOSX環境

*nixとMacユーザにとってはソースからビルドするのが最善の方法でしょう｡
ビルド方法はその最新のバージョン番号と共に <http://redis.io/download> で手に入ります｡
この稿を書いている時点での最新版は2.4.6です｡
このバージョンをインストールするには以下のようにします｡

	wget http://redis.googlecode.com/files/redis-2.4.6.tar.gz
	tar xzf redis-2.4.6.tar.gz
	cd redis-2.4.6
	make

(別の方法として､Redisはパッケージマネージャ経由でインストールすることも出来ます｡例えばMacOSユーザは `brew install redis` と打つことでインストールできます)

ソースからビルドした場合は､コンパイルされたバイナリは `src` ディレクトリに置かれます｡ `src` ディレクトリに移動するには `cd src` を実行します｡

### Redisを起動し､接続する

全てが上手くいったら､redisバイナリが利用可能になっているはずです｡Redisは便利な実行ファイルを持っています｡
それらのうち､RedisサーバとRedisコマンドラインインターフェイスに焦点を当てます｡
まずサーバを実行しましょう｡Windowsでは `redis-server` をダブルクリック､ *nixかMacOSXでは `./redis-server` を実行です｡

スタートアップメッセージを読むと､ `redis.conf` ファイルが見つからないという警告が出ているのに気がつくと思います｡Redisはデフォルト設定で起動しています｡これからの演習にはデフォルト設定で十分です｡

次にRedisコンソールを立ち上げます｡Windowsでは `redis-cli` をダブルクリック､*nix/MacOSXでは `./redis-cli`を実行です｡これでローカルで実行されているサーバにポート6379でつながります｡

コマンドラインに `info` と入力することでRedisがきちんと動いているかどうか確かめることが出来ます｡
正常に動いていればサーバの稼働状況を示す大量のキーと値のペを目にすることでしょう｡

ここまででなにか問題があれば [公式Redisサポートグループ](https://groups.google.com/forum/#!forum/redis-db)　で助けを求めることをお勧めします｡

## Redis Drivers

これから見ていくように､RedisのAPIは関数の集合として表現されます｡
APIは非常に単純で順序だっており､これはつまり､コマンドラインツールを使おうが､好みの言語のドライバを使おうが､やることはほとんど同じだということです｡
したがって以降の内容を好みのプログラミング言語で実行しても何ら問題はないでしょう｡
もし必要があれば [client page](http://redis.io/clients) へ行ってドライバをダウンロードしてください｡
\clearpage

## Chapter 1 - 基本

なにがRedisを際立たせているのでしょうか？Redisはどんなタイプの問題を解決するのでしょうか？Redisを使う際に開発者が気をつけるべきことは何でしょうか？これらの質問に答える前に､まずRedisとはなにかを理解しなければなりません｡

Redisは良くインメモリ パーシスタント キーバリューストアと表現されます｡私はこれはあまり正確な表現だとは思いません｡Redisは全てのデータをメモリ上に持ち(これについては後ほど詳しく触れます)､そのデータを永続化のためにディスクに書きだします｡しかし､Redisは単なるキーバリューストア以上の存在です｡この誤解から離れることは重要です｡さもなければRedisの役割や解決できる問題はごく少ないものになってしまうでしょう｡

実際にはRedisは5つの異なったデータ構造を提供しており､それらのうち一つだけが､典型的なキーバリューストア構造です｡5つのデータ構造と､それらがどのように動き､どんなメソッドを提供し､データモデリングにどう使えるかを理解することがRedisを理解する鍵です｡しかしまず初めにデータ構造について思いを巡らせてみましょう｡

データ構造の考え方をリレーショナルデータベースにあてはめると､データベースは一つのデータ構造だけを提供します｡それはテーブルです｡テーブルは行と列からなり､柔軟です｡モデルを作ったり､格納したり､データを操作したりとテーブルについてできないことはほとんどありません｡しかし欠点もあります｡それは､何もかもが単純ではなく､速くもない､ということです｡本来はそうあるべきなのに｡もし一つのデータ構造で何でもかんでもやろうとするのではなく､より特化した構造が使えたとしたらどうでしょう？確かにやれないこともでてくるでしょうが､単純さと速さを得ることができるのではないでしょうか？

ある問題に適したデータ構造を使うというのは我々がプログラムを書くときにやっていることではないでしょうか？
ハッシュテーブルやスカラー変数をあらゆるデータに使ったりはしないでしょう｡Redisのやり方というのはそういう事です｡
スカラーやリストやハッシュや集合を扱うなら､それをそのまま保存すればいいのではないでしょうか？
どうして値の存在を確認するために `exists(key)` 以上の複雑なことをしなければならないのでしょう？
あるいは O(1)(項目数に関わらず一定の時間で検索できること)以上の時間のかかることをしなければいけないのでしょう？

## ブロックを積み上げる

### データベース

Redisは既にあなたにとって馴染みのあるデータベースの概念と同じ基本的な考え方を持っています｡
データベースはデータの集合を保持します｡データベースの典型的な利用例はアプリケーションのデータをひとまとめにして他のアプリケーションから分離することです｡

Redisにおいては､データベースは単なる数字で認識されます｡デフォルトのデータベースは `0` です｡別のデータベースに変更したいなら､`select`コマンドで変更できます｡コマンドラインインターフェイスで `select 1` と入力してみましょう｡Redisが `OK` メッセージを返し､プロンプトが `redis 127.0.0.1:6379[1]` のように変わるはずです｡デフォルトのデータベースに戻りたければ､コマンドラインインターフェイスで `select 0` と入力してください｡

### コマンドとキーと値

Redisは単なるキーバリューストア以上の存在ですが､本質的にはRedisの5つのデータ構造は全てキーと値を持ちます｡
より深く学ぶ前に､まずキーと値について理解することが必要です｡

キーはデータを識別する方法です｡キーについては後ほど十分に取り扱いますので､今のところはキーが `users:leto` のような見た目をしていると知っていれば十分です｡このようなキーが `leto` という名前のユーザについての情報を格納しているということは容易に推測できるでしょう｡
Redisにとってコロンは何ら特別な意味を持ちません｡しかしキーの構成にセパレータを用いることは一般的です｡

値はキーに関連付けられた実際のデータのことです｡
値はどんな形でも構いません｡
文字列を入れることもあれば､整数を入れることもあり､また時にはシリアライズされたオブジェクトのこともあるでしょう(JSONとかXMLとか)｡
大抵の場合､Redisは値を単なるバイト列として取り扱い､中身には頓着しません｡しかし､ドライバによっては知り荒いぜーションの取り扱いに差異があることには注意してください｡
この本では文字列と整数とJSONだけを取り扱うことにします｡

すこし手を動かしましょう､次のコマンドを入力してください：

	set users:leto "{name: leto, planet: dune, likes: [spice]}"

これがRedisのコマンドの基本です｡まずコマンドを指定します､次に上の例では`set`です｡次に引数がきます｡
`set`コマンドは2つの引数を取ります｡
これから設定しようとするキーと､キーに設定する値です｡
全てではありませんがほとんどの場合コマンドはキーを引数に取り､その場合たいていキーが最初にきます｡
今設定した値を取り出すにはどうすればいいかわかりますか？
お分かりだと思いますが､わからなくても心配ありません｡

	get users:leto

他の組み合わせも試してみてください｡キーと値は基本となる概念です｡
そして`get`コマンドと`set`コマンドはキーと値を扱う一番単純な方法です｡
もっとユーザを作り､違ったキーを試し､違った値を入れてみてください｡

### クエリ

次に2つのことを明らかにしましょう｡
ことRedisにおいては､キーが全てであり､値には意味がありません｡
別の言い方をすれば､Redisでは値についてクエリを投げることはできません｡
上の例で言えば､惑星`dune`に住むユーザを見つけることはできないのです｡

多くの人にとってこれは困ったことに思えるでしょう｡
我々はデータクエリが非常に柔軟で､非常に強力な世界に住んでいます｡
そのためRedisのやり方は原始的で実際的ではないように思えるでしょう｡
しかしなにも心配することはありません｡思い出してください､Redisは万能のソリューションではありません｡
クエリの制限によってRedisには適さない問題というのもあるのです｡
また､場合によってはデータをモデリングする別の方法を見つけることもあるでしょう｡

これからもっと具体的な例を見ていくことにしますが､
この基本的事実を理解しておくことは重要です｡
これによって､なぜ値は何でもありなのかが理解できます｡
なぜならRedisは決して値を読んだり理解したりする必要がないからです｡
また､この新しい考え方でデータをモデリングする際の助けにもなります｡

### メモリと永続化

先ほどRedisはインメモリ パーシスタント ストア だといいました｡
永続化の点について言えば､デフォルトではRedisは変更されたキーの数に基づいてデータベースのスナップショットをディスクに保存します｡
X個のキーが変更されたら､Y秒ごとにデータベースを保存するという設定ができます｡
デフォルトでは1000以上のキーが変更された場合は60秒ごとに､9あるいはそれ以下のキーが変更された場合には15分おきにデータベースを保存します｡

別の方法として､あるいはスナップショットと併せて､Redisは追加モードで動かすことができます｡キーが追加／変更される度にディスク上のappend-onlyファイルが更新されます｡
ハードウェアなりソフトウェアなりの障害はつきものです｡パフォーマンスを得るために60秒間のデータを失ってもいいb場合というのはあるでしょう｡
しかしそのようなデータの喪失が許されないこともあります｡
Redisではこの2つを選ぶことができます｡第5章では3つ目の選択として永続化作業をスレーブに任せる方法を紹介します｡

メモリに関して言えば､Redisは全てのデータをメモリ上に保持します｡これはRedisを動かすのにはお金がかかるということを意味します｡RAMはいまだにサーバの最も高価なパーツです｡

私はデータが取るスペースどれほど少ないものかを理解していない開発者がいると感じます｡
シェイクスピアの全作品はおおよそ5.5MBです｡
スケーリングについては､他のソリューションはIOバウンドかCPUバウンドしがちです｡
RAMかIOの限界によってたくさんのマシンへとスケールアウト必要があるかどうかは､取り扱うデータタイプに強く依存します｡
もし巨大なマルチメディアファイルをRedisに保存するのでなければ､メモリについてはおそらく問題にはなりません｡
アプリケーションにとってはメモリバウンドよりもIOバウンドのほうが問題になりがちでしょう｡

Redisはバーチャルメモリをサポートしています｡しかし､この機能はRedisの開発者たちに失敗だとみなされています｡いずれ廃止されることになるでしょう｡

(付け足しとして､5.5MBのシェイクスピア全集は約2MBに圧縮することができます｡Redisは自動で圧縮を行いませんが､値はバイト列なので､あなた自身がCPU時間を使って圧縮／解凍を行い､RAMを節約できない理由はなにもありません｡)

### ここまでのまとめ

ここまで多くの概念について触れてきました｡具体的なRedisの使い方に踏み込む前に一度これらをまとめておきたいと思います｡
とりわけクエリの制限､データ構造､そしてRedis流メモリへのデータ保存の方法についてです｡

これら3つを組み合わせると素晴らしい物を得られます｡それは速さです｡
「そりゃ速いだろうさ､全部メモリ上にあるんだから」と思う人が居るでしょうが､それは速さの一部分にすぎません｡
Redisが他のデータベースと比較して真に輝くのはその特別なデータ構造によってです｡

一体どれほど速いのでしょう？それは多くの要因に依ります｡
使うコマンド､データタイプ､そういったことです｡
しかしRedisの速度は「一秒に」何万とか､何十万とかの単位で測定されます｡
これは `redis-benchmark` コマンド(redis-serverやredis-cliと同じフォルダにあります)を実行することでユーザ自身が確認できます｡

私はかつてリレーショナルデータベースをRedisに変更したことがあります｡
RDBでのロードテストには5分以上かかりましたが､Redisでは150ミリ秒で終わりました｡

このRedisの性質を理解することは重要です｡なぜならそれがRedisの使い方に大きな影響を与えるからです｡
SQLを使い慣れた開発者はデータベースへの問い合わせをできるだけ少なくしようとします｡
Redisを含むあらゆるシステムにとってそれはいいアドバイスです｡
しかしながらよりシンプルなデータ構造を扱う前提に立つと､
目的を達するために複数回Redisにアクセスする必要が生じることがあります｡
このようなデータアクセスパターンは最初は不自然に感じられるかもしれません｡
しかし現実には実際に得られるパフォーマンスに比べるとごく僅かなコストで済むことが多いのです｡

### この章のまとめ

ほとんどRedisそのものには触っていませんが､たくさんの話題について触れました｡
何かわからないこと､例えばクエリとか､があっても心配しないでください｡
次の章で実際に手を動かすことで､疑問が氷解することでしょう｡

この章のポイント:

* キーはデータ(値)を識別する文字列

* 値は任意のバイト列で､Redisはその中身に一切関知しない

* Redisは5つのデータ構造を提供する

* 以上のことから､Redisは高速で容易に扱えるが､全ての課題に最適なわけではない｡
\clearpage

## 第二章 - データ構造

Redisの5つのデータ構造について見ていきましょう｡
それぞれのデータ構造について解説し､
どんなメソッドが利用可能で､
どんな機能やデータが適するのか見てきます｡

これまでに見てきたRedisの要素はコマンドとキーと値だけでした｡
データ構造については詳しく触れていません｡
`set`コマンドを使うときに､Redisはどうやってどのデータ構造を使うか判別するのでしょうか？
実は全てのコマンドはデータ構造と結びついているのです｡
たとえば`set`コマンドを使う時､格納される値は文字列です｡
`hset`コマンドを使うとハッシュとして格納します｡
Redisのコマンド群に少し馴染めば実に扱いやすくできています｡

**[Redisのウェブサイト](http://redis.io/commands) には大変良く出来たリファレンスがあります｡
同じ事を繰り返す必要はないので､データ構造の目的を理解するのに必要な最も重要なコマンドだけを取り上げます｡**

実際に物事に取り組んで楽しむことほど大事なことはありません｡
`flushdb`コマンドでいつでもデータベースを全消去できるので､
ためらわずに思う存分試してみてください｡

### 文字列

文字列はRedisで一番基本的なデータ構造です｡
一般にキーと値のペアというと文字列のことを思い浮かべるでしょう｡
しかし名前に惑わされないでください､値は何だっていいのです｡
私はそれらをスカラー値と呼ぶのが好きですが､たぶんこれは私だけでしょう｡

文字列の使い方については既に見ました｡オブジェクトのインスタンスをキーによって保存します｡
次のような使い方を頻繁にすることでしょう：

	set users:leto "{name: leto, planet: dune, likes: [spice]}"

加えて､いくつかありふれた操作も行えます｡たとえば `strlen <key>` とすると､キーが示す値の長さを得られます｡`getrange <key> <start> <end>` は値の指定した範囲を返します｡
`append <key>` は既にあるキーに値を追加するか､キーが存在しなければ新しく作成して追加します｡
実際にやってみて下さい｡次のような結果になります：

	> strlen users:leto
	(integer) 42

	> getrange users:leto 27 40
	"likes: [spice]"

	> append users:leto " OVER 9000!!"
	(integer) 54

さて､あなたはこれを見て「なるほどわかった､しかしこんなのは意味がない」と考えているのではないでしょうか｡
JSONから範囲指定で値を取り出したり､値を追加したりすることには意味がありません｡ここでは文字列に限ったコマンドを実行する練習をしているだけです｡とうぜん文字列を扱う場合にしか意味がありません｡

先ほどRedisは値について関知しないといいました｡
大抵の場合はそれは本当のことですが､幾つかもの文字列コマンドは値や構造に特化しています｡
漠然とした例ですが､`append`とか`getrange`とかは効率のいい知り荒いぜーションをするのに便利です｡
より具体的な例としては`incr` `incrby` `decr` `decrby` コマンドがあります｡
これらは文字列の値を増やしたり減らしたりします｡

	> incr stats:page:about
	(integer) 1
	> incr stats:page:about
	(integer) 2

	> incrby ratings:video:12333 5
	(integer) 5
	> incrby ratings:video:12333 3
	(integer) 8

ご想像のとおり､Redisの文字列は分析に大変役に立ちます｡
試しに整数値ではない `users:leto` をインクリメント仕様としてみてください､エラーになるはずです｡

より進んだ例として `setbit` `getbit` コマンドがあります｡
[素晴らしい解説](http://blog.getspool.com/2011/11/29/fast-easy-realtime-metrics-using-redis-bitmaps/)
があります｡Spoolがこれらのコマンドを使ってどれほど効率的に一日のユニークビジター数を計算しているかの例です｡
1億2800万ユーザを一台のラップトップで､50ミリ秒と16MBのメモリで処理しています｡

ビットマップの仕組みを理解することは重要ではありません｡
同様にSpoolがどんなふうに使っているかというのも重要ではありません｡しかし
Redisの文字列が見た目よりずっと強力だということは理解する必要があります｡
しかし最も一般的な使われれ方は上に上げたようなオブジェクトを格納するとか､カウンターを作るとか､そういったことです｡
またキーから値を得るのは大変速いので､文字列はよくキャッシュに使われます｡

### ハッシュ

なぜRedisが単なるキーバリューストアではないのか､その好例がハッシュです｡
多くの点でハッシュは文字列と似通っています｡重要な違いはキーと値の間に追加要素があることです｡
それはフィールドです｡
したがって､ハッシュにおける `set` `get` は次のようになります：

	hset users:goku powerlevel 9000
	hget users:goku powerlevel

複数のフィールドを一度に設定したり､取得したりすることもできます｡
全てのフィールドを表示したり､特定のフィールドを削除するには次のようにします：

	hmset users:goku race saiyan age 737
	hmget users:goku race powerlevel
	hgetall users:goku
	hkeys users:goku
	hdel users:goku age

見てわかるように､ハッシュは単なる文字列より扱いやすくなっています｡
ユーザ情報をシリアライズされた文字列として保存するより､ハッシュで保存したほうが理解が容易です｡
利点は値全部を読み書きするのではなく､値の特定の部分を取り出したり更新したり削除したりできることです｡

具体的なモデル､例えばユーザ情報とかを使ってハッシュを見ることが理解の鍵です｡
そしてパフォーマンスの観点から､より細かなコントロールが有益です｡
しかし次の章ではデータ構造化やクエリを行うのにハッシュをどのように利用できるのか見て行きます｡
個人的にはハッシュが最もその効果を発揮する場面です｡


### リスト

リストはキーに対して値の配列を格納します｡
リストに対して値を追加し､最初や最後の値を取得し､インデックスを使ってリスト内の値を操作できます｡
リストは内部に順序を保持しており､効率の良いインデックスベースの操作が可能です｡
`newusers`リストを使って新しくサイトに登録したユーザを追跡できます：

	lpush newusers goku
	ltrim newusers 0 50

まず新しいユーザをリストの先頭に追加します｡
次に最後の50ユーザだけを残して他を削除しています｡
これが一般的なパターンです｡
`ltrim`はO(N)操作です｡Nは削除する要素の数です｡
この例の場合､一回の追加の後にトリムします｡したがって常にO(1)の時間がかかることになります｡


次はあるキーの値を元に別のキーの値を参照するという初めての例です｡
もし最新10ユーザの詳細が知りたければ､次の操作の組み合わせで可能です：

	keys = redis.lrange('newusers', 0, 10)
	redis.mget(*keys.map {|u| "users:#{u}"})

このコードはRubyの例です｡以前述べた複数回の問い合わせを行っています｡

Of course, lists aren't only good for storing references to other keys. The values can be anything. You could use lists to store logs or track the path a user is taking through a site. If you were building a game, you might use it to track a queued user actions.

### Sets

Set are used to store unique values and provide a number of set-based operations, like unions. Sets aren't ordered but they provide efficient value-based operations. A friend's list is the classic example of using a set:

	sadd friends:leto ghanima paul chani jessica
	sadd friends:duncan paul jessica alia

Regardless of how many friends a user has, we can efficiently tell (O(1)) whether userX is a friend of userY or not

	sismember friends:leto jessica
	sismember friends:leto vladimir

Furthermore we can see what two or more people share the same friends:

	sinter friends:leto friends:duncan

and even store the result at a new key:

	sinterstore friends:leto_duncan friends:leto friends:duncan

Sets are great for tagging or tracking any other properties of a value for which duplicates don't make any sense (or where we want to apply set operations such as intersections and unions).

### Sorted Sets

The last and most powerful data structure are sorted sets. If hashes are like strings but with fields, then sorted sets are like sets but with a score. The score provides sorting and ranking capabilities. If we wanted a ranked list of friends, we might do:

	zadd friends:duncan 70 ghanima 95 paul 95 chani 75 jessica 1 vladimir

Want to find out how many friends `duncan` has with a rank of 90 or over?

	zcount friends:duncan 90 100

How about figuring out `chani`'s rank?

	zrevrank friends:duncan chani

We use `zrevrank` instead of `zrank` since Redis' default sort is from low to high (but in this case we are ranking from high to low). The most obvious use-case for sorted sets is a leaderboard system. In reality though, anything you want sorted by an some integer, and be able to efficiently manipulate based on that score, might be a good fit for a sorted set.

### In This Chapter

That's a high level overview of Redis' five data structures. One of the neat things about Redis is that you can often do more than you first realize. There are probably ways to use string and sorted sets that no one has thought of yet. As long as you understand the normal use-case though, you'll find Redis ideal for all types of problems. Also, just because Redis exposes five data structures and various methods, don't think you need to use all of them. It isn't uncommon to build a feature while only using a handful of commands.

\clearpage

## Chapter 3 - Leveraging Data Structures

In the previous chapter we talked about the five data structures and gave some examples of what problems they might solve. Now it's time to look at a few more advanced, yet common, topics and design patterns.

### Big O Notation

Throughout this book we've made references to the Big O notation in the form of O(n) or O(1). Big O notation is used to explain how something behaves given a certain number of elements. In Redis, it's used to tell us how fast a command is based on the number of items we are dealing with.

Redis documentation tells us the Big O notation for each of its commands. It also tells us what the factors are that influence the performance. Let's look at some examples.

The fastest anything can be is O(1) which is a constant. Whether we are dealing with 5 items or 5 million, you'll get the same performance. The `sismember` command, which tells us if a value belongs to a set, is O(1). `sismember` is a powerful command, and its performance characteristics are a big reason for that. A number of Redis commands are O(1).

Logarithmic, or O(log(N)), is the next fastest possibility because it needs to scan through smaller and smaller partitions. Using this type of divide and conquer approach, a very large number of items quickly gets broken down in a few iterations. `zadd` is a O(log(N)) command, where N is the number of elements already in the set.

Next we have linear commands, or O(N). Looking for a non-indexed column in a table is an O(N) operation. So is using the `ltrim` command. However, in the case of `ltrim`, N isn't the number of elements in the list, but rather the elements being removed. Using `ltrim` to remove 1 item from a list of millions will be faster than using `ltrim` to remove 10 items from a list of thousands. (Though they'll probably both be so fast that you wouldn't be able to time it.)

`zremrangebyscore` which removes elements from a sorted set with a score between a minimum and a maximum value has a complexity of O(log(N)+M). This makes it a mix. By reading the documentation we see that N is the number of total elements in the set and M is the number of elements to be removed. In other words, the number of elements that'll get removed is probably going to be more significant, in terms of performance, than the total number of elements in the list.

The `sort` command, which we'll discuss in greater detail in the next chapter has a complexity of O(N+M*log(M)). From its performance characteristic, you can probably tell that this is one of Redis' most complex commands.

There are a number of other complexities, the two remaining common ones are O(N^2) and O(C^N). The larger N is, the worse these perform relative to a smaller N. None of Redis' commands have this type of complexity.

It's worth pointing out that the Big O notation deals with the worst case. When we say that something takes O(N), we might actually find it right away or it might be the last possible element.


### Pseudo Multi Key Queries

A common situation you'll run into is wanting to query the same value by different keys. For example, you might want to get a user by email (for when they first log in) and also by id (after they've logged in). One horrible solution is to duplicate your user object into two string values:

	set users:leto@dune.gov "{id: 9001, email: 'leto@dune.gov', ...}"
	set users:9001 "{id: 9001, email: 'leto@dune.gov', ...}"

This is bad because it's a nightmare to manage and it takes twice the amount of memory.

It would be nice if Redis let you link one key to another, but it doesn't (and it probably never will). A major driver in Redis' development is to keep the code and API clean and simple. The internal implementation of linking keys (there's a lot we can do with keys that we haven't talked about yet) isn't worth it when you consider that Redis already provides a solution: hashes.

Using a hash, we can remove the need for duplication:

	set users:9001 "{id: 9001, email: leto@dune.gov, ...}"
	hset users:lookup:email leto@dune.gov 9001

What we are doing is using the field as a pseudo secondary index and referencing the single user object. To get a user by id, we issue a normal `get`:

	get users:9001

To get a user by email, we issue an `hget` followed by a `get` (in Ruby):

	id = redis.hget('users:lookup:email', 'leto@dune.gov')
	user = redis.get("users:#{id}")

This is something that you'll likely end up doing often. To me, this is where hashes really shine, but it isn't an obvious use-case until you see it.

### References and Indexes

We've seen a couple examples of having one value reference another. We saw it when we looked at our list example, and we saw it in the section above when using hashes to make querying a little easier. What this comes down to is essentially having to manually manage your indexes and references between values. Being honest, I think we can say that's a bit of a downer, especially when you consider having to manage/update/delete these references manually. There is no magic solution to solving this problem in Redis.

We already saw how sets are often used to implement this type of manual index:

	sadd friends:leto ghanima paul chani jessica

Each member of this set is a reference to a Redis string value containing details on the actual user. What if `chani` changes her name, or deletes her account? Maybe it would make sense to also track the inverse relationships:

	sadd friends_of:chani leto paul

Maintenance cost aside, if you are anything like me, you might cringe at the processing and memory cost of having these extra indexed values. In the next section we'll talk about ways to reduce the performance cost of having to do extra round trips (we briefly talked about it in the first chapter).

If you actually think about it though, relational databases have the same overhead. Indexes take memory, must be scanned or ideally seeked and then the corresponding records must be looked up. The overhead is neatly abstracted away (and they  do a lot of optimizations in terms of the processing to make it very efficient).

Again, having to manually deal with references in Redis is unfortunate. But any initial concerns you have about the performance or memory implications of this should be tested. I think you'll find it a non-issue.

### Round Trips and Pipelining

We already mentioned that making frequent trips to the server is a common pattern in Redis. Since it is something you'll do often, it's worth taking a closer look at what features we can leverage to get the most out of it.

First, many commands either accept one or more set of parameters or have a sister-command which takes multiple parameters. We saw `mget` earlier, which takes multiple keys and returns the values:

	keys = redis.lrange('newusers', 0, 10)
	redis.mget(*keys.map {|u| "users:#{u}"})

Or the `sadd` command which adds 1 or more members to a set:

	sadd friends:vladimir piter
	sadd friends:paul jessica leto "leto II" chani

Redis also supports pipelining. Normally when a client sends a request to Redis it waits for the reply before sending the next request. With pipelining you can send a number of requests without waiting for their responses. This reduces the networking overhead and can result in significant performance gains.

It's worth noting that Redis will use memory to queue up the commands, so it's a good idea to batch them. How large a batch you use will depend on what commands you are using, and more specifically, how large the parameters are. But, if you are issuing commands against ~50 character keys, you can probably batch them in thousands or tens of thousands.

Exactly how you execute commands within a pipeline will vary from driver to driver. In Ruby you pass a block to the `pipelined` method:

	redis.pipelined do
	  9001.times do
		redis.incr('powerlevel')
	  end
	end

As you can probably guess, pipelining can really speed up a batch import!

### Transactions

Every Redis command is atomic, including the ones that do multiple things. Additionally, Redis has support for transactions when using multiple commands.

You might not know it, but Redis is actually single-threaded, which is how every command is guaranteed to be atomic. While one command is executing, no other command will run. (We'll briefly talk about scaling in a later chapter.) This is particularly useful when you consider that some commands do multiple things. For example:

`incr` is essentially a `get` followed by a `set`

`getset` sets a new value and returns the original

`setnx` first checks if the key exists, and only sets the value if it does not

Although these commands are useful, you'll inevitably need to run multiple commands as an atomic group. You do so by first issuing the `multi` command, followed by all the commands you want to execute as part of the transaction, and finally executing `exec` to actually execute the commands or `discard` to throw away, and not execute the commands. What guarantee does Redis make about transactions?

* The commands will be executed in order

* The commands will be executed as a single atomic operation (without another client's command being executed halfway through)

* That either all or none of the commands in the transaction will be executed

You can, and should, test this in the command line interface. Also note that there's no reason why you can't combine pipelining and transactions.

	multi
	hincrby groups:1percent balance -9000000000
	hincrby groups:99percent balance 9000000000
	exec

Finally, Redis lets you specify a key (or keys) to watch and conditionally apply a transaction if the key(s) changed. This is used when you need to get values and execute code based on those values, all in a transaction. With the code above, we wouldn't be able to implement our own `incr` command since they are all executed together once `exec` is called. From code, we can't do:

	redis.multi()
	current = redis.get('powerlevel')
	redis.set('powerlevel', current + 1)
	redis.exec()

That isn't how Redis transactions work. But, if we add a `watch` to `powerlevel`, we can do:

	redis.watch('powerlevel')
	current = redis.get('powerlevel')
	redis.multi()
	redis.set('powerlevel', current + 1)
	redis.exec()

If another client changes the value of `powerlevel` after we've called `watch` on it, our transaction will fail. If no client changes the value, the set will work. We can execute this code in a loop until it works.

### Keys Anti-Pattern

In the next chapter we'll talk about commands that aren't specifically related to data structures. Some of these are administrative or debugging tools. But there's one I'd like to talk about in particular: the `keys` command. This command takes a pattern and finds all the matching keys. This command seems like it's well suited for a number of tasks, but it should never be used in production code. Why? Because it does a linear scan through all the keys looking for matches. Or, put simply, it's slow.

How do people try and use it? Say you are building a hosted bug tracking service. Each account will have an `id` and you might decide to store each bug into a string value with a key that looks like `bug:account_id:bug_id`. If you ever need to find all of an account's bugs (to display them, or maybe delete them if they delete their account), you might be tempted (as I was!) to use the `keys` command:

	keys bug:1233:*

The better solution is to use a hash. Much like we can use hashes to provide a way to expose secondary indexes, so too can we use them to organize our data:

	hset bugs:1233 1 "{id:1, account: 1233, subject: '...'}"
	hset bugs:1233 2 "{id:2, account: 1233, subject: '...'}"

To get all the bug ids for an account we simply call `hkeys bugs:1233`. To delete a specific bug we can do `hdel bugs:1233 2` and to delete an account we can delete the key via `del bugs:1233`.


### In This Chapter

This chapter, combined with the previous one, has hopefully given you some insight on how to use Redis to power real features. There are a number of other patterns you can use to build all types of things, but the real key is to understand the fundamental data structures and to get a sense for how they can be used to achieve things beyond your initial perspective.

\clearpage

## Chapter 4 - Beyond The Data Structures

While the five data structures form the foundation of Redis, there are other commands which aren't data structure specific. We've already seen a handful of these: `info`, `select`, `flushdb`, `multi`, `exec`, `discard`, `watch` and `keys`. This chapter will look at some of the other important ones.

### Expiration

Redis allows you to mark a key for expiration. You can give it an absolute time in the form of a Unix timestamp (seconds since January 1, 1970) or a time to live in seconds. This is a key-based command, so it doesn't matter what type of data structure the key represents.

	expire pages:about 30
	expireat pages:about 1356933600

The first command will delete the key (and associated value) after 30 seconds. The second will do the same at 12:00 a.m. December 31st, 2012.

This makes Redis an ideal caching engine. You can find out how long an item has to live until via the `ttl` command and you can remove the expiration on a key via the `persist` command:

	ttl pages:about
	persist pages:about

Finally, there's a special string command, `setex` which lets you set a string and specify a time to live in a single atomic command (this is more for convenience than anything else):

	setex pages:about 30 '<h1>about us</h1>....'

### Publication and Subscriptions

Redis lists have an `blpop` and `brpop` command which returns and removes the first (or last) element from the list or blocks until one is available. These can be used to power a simple queue.

Beyond this, Redis has first-class support for publishing messages and subscribing to channels. You can try this out yourself by opening a second `redis-cli` window. In the first window subscribe to a channel (we'll call it `warnings`):

	subscribe warnings

The reply is the information of your subscription. Now, in the other window, publish a message to the `warnings` channel:

	publish warnings "it's over 9000!"

If you go back to your first window you should have received the message to the `warnings` channel.

You can subscribe to multiple channels (`subscribe channel1 channel2 ...`), subscribe to a pattern of channels (`psubscribe warnings:*`) and use the `unsubscribe` and `punsubscribe` commands to stop listening to one or more channels, or a channel pattern.

Finally, notice that the `publish` command returned the value 1. This indicates the number of clients that received the message.


### Monitor and Slow Log

The `monitor` command lets you see what Redis is up to. It's a great debugging tool that gives you insight into how your application is interacting with Redis. In one of your two redis-cli windows (if one is still subscribed, you can either use the `unsubscribe` command or close the window down and re-open a new one) enter the `monitor` command. In the other, execute any other type of command (like `get` or `set`). You should see those commands, along with their parameters, in the first window.

You should be wary of running monitor in production, it really is a debugging and development tool. Aside from that, there isn't much more to say about it. It's just a really useful tool.

Along with `monitor`, Redis has a `slowlog` which acts as a great profiling tool. It logs any command which takes longer than a specified number of **micro**seconds. In the next section we'll briefly cover how to configure Redis, for now you can configure Redis to log all commands by entering:

	config set slowlog-log-slower-than 0

Next, issue a few commands. Finally you can retrieve all of the logs, or the most recent logs via:

	slowlog get
	slowlog get 10

You can also get the number of items in the slow log by entering `slowlog len`

For each command you entered you should see four parameters:

* An auto-incrementing id

* A Unix timestamp for when the command happened

* The time, in microseconds, it took to run the command

* The command and its parameters

The slow log is maintained in memory, so running it in production, even with a low threshold, shouldn't be a problem. By default it will track the last 1024 logs.

### Sort

One of Redis' most powerful commands is `sort`. It lets you sort the values within a list, set or sorted set (sorted sets are ordered by score, not the members within the set). In its simplest form, it allows us to do:

	rpush users:leto:guesses 5 9 10 2 4 10 19 2
	sort users:leto:guesses

Which will return the values sorted from lowest to highest. Here's a more advanced example:

	sadd friends:ghanima leto paul chani jessica alia duncan
	sort friends:ghanima limit 0 3 desc alpha

The above command shows us how to page the sorted records (via `limit`), how to return the results in descending order (via `desc`) and how to sort lexicographically instead of numerically (via `alpha`).

The real power of `sort` is its ability to sort based on a referenced object. Earlier we showed how lists, sets and sorted sets are often used to reference other Redis objects. The `sort` command can dereference those relations and sort by the underlying value. For example, say we have a bug tracker which lets users watch issues. We might use a set to track the issues being watched:

	sadd watch:leto 12339 1382 338 9338

It might make perfect sense to sort these by id (which the default sort will do), but we'd also like to have these sorted by severity. To do so, we tell Redis what pattern to sort by. First, let's add some more data so we can actually see a meaningful result:

	set severity:12339 3
	set severity:1382 2
	set severity:338 5
	set severity:9338 4

To sort the bugs by severity, from highest to lowest, you'd do:

	sort watch:leto by severity:* desc

Redis will substitute the `*` in our pattern (identified via `by`) with the values in our list/set/sorted set. This will create the key name that Redis will query for the actual values to sort by.

Although you can have millions of keys within Redis, I think the above can get a little messy. Thankfully `sort` can also work on hashes and their fields. Instead of having a bunch of top-level keys you can leverage hashes:

	hset bug:12339 severity 3
	hset bug:12339 priority 1
	hset bug:12339 details "{id: 12339, ....}"

	hset bug:1382 severity 2
	hset bug:1382 priority 2
	hset bug:1382 details "{id: 1382, ....}"

	hset bug:338 severity 5
	hset bug:338 priority 3
	hset bug:338 details "{id: 338, ....}"

	hset bug:9338 severity 4
	hset bug:9338 priority 2
	hset bug:9338 details "{id: 9338, ....}"

Not only is everything better organized, and we can sort by `severity` or `priority`, but we can also tell `sort` what field to retrieve:

	sort watch:leto by bug:*->priority get bug:*->details

The same value substitution occurs, but Redis also recognizes the `->` sequence and uses it to look into the specified field of our hash. We've also included the `get` parameter, which also does the substitution and field lookup, to retrieve bug details.

Over large sets, `sort` can be slow. The good news is that the output of a `sort` can be stored:

	sort watch:leto by bug:*->priority get bug:*->details store watch_by_priority:leto

Combining the `store` capabilities of `sort` with the expiration commands we've already seen makes for a nice combo.


### In This Chapter

This chapter focused on non-data structure-specific commands. Like everything else, their use is situational. It isn't uncommon to build an app or feature that won't make use of expiration, publication/subscription and/or sorting. But it's good to know that they are there. Also, we only touched on some of the commands. There are more, and once you've digested the material in this book it's worth going through the [full list](http://redis.io/commands).

\clearpage

## Chapter 5 - Administration

Our last chapter is dedicated to some of the administrative aspects of running Redis. In no way is this a comprehensive guide on Redis administration. At best we'll answer some of the more basic questions new users to Redis are most likely to have.

### Configuration

When you first launched the Redis server, it warned you that the `redis.conf` file could not be found. This file can be used to configure various aspects of Redis. A well-documented `redis.conf` file is available for each release of Redis. The sample file contains the default configuration options, so it's useful to both understand what the settings do and what their defaults are. You can find it at <https://github.com/antirez/redis/raw/2.4.6/redis.conf>.

**This is the config file for Redis 2.4.6. You should replace "2.4.6" in the above URL with your version. You can find your version by running the `info` command and looking at the first value.**

Since the file is well-documented, we won't be going over the settings.

In addition to configuring Redis via the `redis.conf` file, the `config set` command can be used to set individual values. In fact, we already used it when setting the `slowlog-log-slower-than` setting to 0.

There's also the `config get` command which displays the value of a setting. This command supports pattern matching. So if we want to display everything related to logging, we can do:

	config get *log*

### Authentication

Redis can be configured to require a password. This is done via the `requirepass` setting (set through either the `redis.conf` file or the `config set` command). When `requirepass` is set to a value (which is the password to use), clients will need to issue an `auth password` command.

Once a client is authenticated, they can issue any command against any database. This includes the `flushall` command which erases every key from every database. Through the configuration, you can rename commands to achieve some security through obfuscation:

	rename-command CONFIG 5ec4db169f9d4dddacbfb0c26ea7e5ef
	rename-command FLUSHALL 1041285018a942a4922cbf76623b741e

Or you can disable a command by setting the new name to an empty string.

### Size Limitations

As you start using Redis, you might wonder "how many keys can I have?" You might also wonder how many fields can a hash have (especially when you use it to organize your data), or how many elements can lists and sets have? Per instance, the practical limits for all of these is in the hundreds of millions.


### Replication

Redis supports replication, which means that as you write to one Redis instance (the master), one or more other instances (the slaves) are kept up-to-date by the master. To configure a slave you use either the `slaveof` configuration setting or the `slaveof` command (instances running without this configuration are or can be masters).

Replication helps protect your data by copying to different servers. Replication can also be used to improve performance since reads can be sent to slaves. They might respond with slightly out of date data, but for most apps that's a worthwhile tradeoff.

Unfortunately, Redis replication doesn't yet provide automated failover. If the master dies, a slave needs to be manually promoted. Traditional high-availability tools that use heartbeat monitoring and scripts to automate the switch are currently a necessary headache if you want to achieve some sort of high availability with Redis.

### Backups

Backing up Redis is simply a matter of copying Redis' snapshot to whatever location you want (S3, FTP, ...). By default Redis saves its snapshop to a file named `dump.rdb`. At any point in time, you can simply `scp`, `ftp` or `cp` (or anything else) this file.

It isn't uncommon to disable both snapshotting and the append-only file (aof) on the master and let a slave take care of this. This helps reduce the load on the master and lets you set more aggressive saving parameters on the slave without hurting overall system responsiveness.

### Scaling and Redis Cluster

Replication is the first tool a growing site can leverage. Some commands are more expensive than others (`sort` for example) and offloading their execution to a slave can keep the overall system responsive to incoming queries.

Beyond this, truly scaling Redis comes down to distributing your keys across multiple Redis instances (which could be running on the same box, remember, Redis is single-threaded). For the time being, this is something you'll need to take care of (although a number of Redis drivers do provide consistent-hashing algorithms). Thinking about your data in terms of horizontal distribution isn't something we can cover in this book. It's also something you probably won't have to worry about for a while, but it's something you'll need to be aware of regardless of what solution you use.

The good news is that work is under way on Redis Cluster. Not only will this offer horizontal scaling, including rebalancing, but it'll also provide automated failover for high availability.

High availability and scaling is something that can be achieved today, as long as you are willing to put the time and effort into it. Moving forward, Redis Cluster should make things much easier.

### In This Chapter

Given the number of projects and sites using Redis already, there can be no doubt that Redis is production-ready, and has been for a while. However, some of the tooling, especially around security and availability is still young. Redis Cluster, which we'll hopefully see soon, should help address some of the current management challenges.

\clearpage

## Conclusion

In a lot of ways, Redis represents a simplification in the way we deal with data. It peels away much of the complexity and abstraction available in other systems. In many cases this makes Redis the wrong choice. In others it can feel like Redis was custom-built for your data.

Ultimately it comes back to something I said at the very start: Redis is easy to learn. There are many new technologies and it can be hard to figure out what's worth investing time into learning. When you consider the real benefits Redis has to offer with its simplicity, I sincerely believe that it's one of the best investments, in terms of learning, that you and your team can make.
