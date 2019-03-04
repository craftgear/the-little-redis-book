## この本について ##
The Little Redis Book はRedisについて紹介したフリーの書籍です｡

この本は [Karl Seguin](http://openmymind.net) によって書かれました｡
[Perry Neal](http://twitter.com/perryneal)がその手伝いをしました｡
( 訳者注：この日本語版は[Shunsuke Watanabe](http://twitter.com/craftgear) が翻訳しました。)

この本が気に入ったなら､ [The Little MongoDB Book](http://openmymind.net/2011/3/28/The-Little-MongoDB-Book/) も気にいるかもしれません｡

## ライセンス ##
この本は [Attribution-NonCommercial 3.0 Unported license](<http://creativecommons.org/licenses/by-nc/3.0/legalcode>) ライセンスのもとで無料で配布されます｡

## 多言語の翻訳 ##

* [ロシア語](https://github.com/kondratovich/the-little-redis-book)
* [イタリア語](https://github.com/sandroconforto/the-little-redis-book) - [pdf](https://github.com/sandroconforto/the-little-redis-book/raw/master/book/redisIt.pdf)
* [中国語](https://github.com/JasonLai256/the-little-redis-book)
* [中国語 2](https://github.com/geminiyellow/the-little-redis-book/)
* [日本語](https://github.com/craftgear/the-little-redis-book/)
* [スペイン語](https://github.com/raulexposito/the-little-redis-book)

## フォーマット ##
この本は [Markdown](http://daringfireball.net/projects/markdown/) で書かれており､ [pandoc](http://johnmacfarlane.net/pandoc/) をつかってPDFに変換されています｡

Tex テンプレートは[Lena Herrmann's JavaScript highlighter](http://lenaherrmann.net/2010/05/20/javascript-syntax-highlighting-in-the-latex-listings-package) を利用しています。

Kindle and ePub format provided using [Pandoc](http://johnmacfarlane.net/pandoc/).

KindleまたはEPUBフォーマットの生成には [pandoc](http://johnmacfarlane.net/pandoc/)を使って下さい。

## 本を出力するには ##
以下に挙げたパッケージはUbuntuの場合に必要なものです。他のOSやディストリビューションの場合、名前が多少違うことがあります。

### PDF

#### 必要パッケージ

* `pandoc`
* `texlive-xetex`
* `texlive-latex-extra`
* `texlive-latex-recommended`

[いくつかのフォント](https://github.com/karlseguin/the-little-redis-book/blob/master/common/pdf-template.tex#L11) も合わせてインストールして下さい。
あるいはこれらのフォントを別のフォントに置き換えることもできます。フォントを変えるとビルドに失敗する可能性があることに留意して下さい。 [ビルド時のトラブル](https://github.com/karlseguin/the-little-redis-book/issues/26).

#### ビルド手順

`make en/redis.pdf` を実行します。

### ePub

#### 必要パッケージ

* `pandoc`

#### ビルド手順

`make en/redis.epub` を実行します。

### Mobi

#### 必要パッケージ

* `pandoc`

合わせて[KindleGen](http://www.amazon.com/gp/feature.html?ie=UTF8&docId=1000765211)のインストールが必要です。

#### ビルド手順

`make en/redis.mobi`を実行します。

## 表紙画像 ##
PDFには表紙画像が含まれています。使用フォントは [Comfortaa](http://www.dafont.com/comfortaa.font) です。
