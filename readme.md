## この本について ##
The Little Redis Book はRedisについて紹介したフリーの書籍です｡

この本は [Karl Seguin](http://openmymind.net) によって書かれました｡
[Perry Neal](http://twitter.com/perryneal)がその手伝いをしました｡

この本が気に入ったなら､ [The Little MongoDB Book](http://openmymind.net/2011/3/28/The-Little-MongoDB-Book/) も気にいるかもしれません｡

## ライセンス ##
この本は [Attribution-NonCommercial 3.0 Unported license](<http://creativecommons.org/licenses/by-nc/3.0/legalcode>) ライセンスのもとで無料で配布されます｡

## 多言語の翻訳 ##

* [ロシア語](https://github.com/kondratovich/the-little-redis-book)
* [イタリア語](https://github.com/sandroconforto/the-little-redis-book) - [pdf](https://github.com/sandroconforto/the-little-redis-book/raw/master/book/redisIt.pdf)

## フォーマット ##
この本は [Markdown](http://daringfireball.net/projects/markdown/) で書かれており､ [pandoc](http://johnmacfarlane.net/pandoc/) をつかってPDFに変換されています｡ いくつかのLaTex独自コマンドがPDF生成のためにMarkdown中で使われています｡（タイトルページやページ送りなど）

PDFまたはKindle､EPUBフォーマットの生成には [pandoc](http://johnmacfarlane.net/pandoc/)をダウンロード＆インストールしてください｡

## PDFの生成法 ##
pandocにはPDFを生成する markdown2pdfが含まれており､ <https://github.com/claes/pandoc-templates>の派生バージョンを使ってPDFを生成するには次のようにします:

	#!/bin/sh
	paper=a4paper
	hmargin=3cm
	vmargin=3cm
	fontsize=11pt

	mainfont=Verdana
	sansfont=Tahoma
	monofont="Courier New"
	columns=onecolumn
	geometry=portrait
	nohyphenation=true


	markdown2pdf --xetex --template=template/xetex.template \
	-V paper=$paper -V hmargin=$hmargin -V vmargin=$vmargin \
	-V mainfont="$mainfont" -V sansfont="$sansfont" -V monofont="$monofont" \
	-V geometry=$geometry -V columns=$columns -V fontsize=$fontsize \
	-V nohyphenation=$nohyphenation en/redis.md -o redis.pdf

## EPUBの生成法 ##
以下のコマンドを使ってください ( <http://news.ycombinator.com/item?id=3502033> からのヒントを得て修正されました):

	pandoc -f markdown -t epub --epub-metadata=en/metadata.xml \
	--template=template/xetex.template -V paper=$paper \
	-V hmargin=$hmargin -V vmargin=$vmargin -V mainfont="$mainfont" \
	-V sansfont="$sansfont" -V monofont="$monofont" -V geometry=$geometry \
	-V columns=$columns -V fontsize=$fontsize -V nohyphenation=$nohyphenation \
	en/redis.md -o redis.epub

## Title Image ##
A PSD of the title image is included. The font used is [Comfortaa](http://www.dafont.com/comfortaa.font).
