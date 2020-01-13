# tcolorboxのライブラリ紹介(ver 4.22版)

2020/1/12(Sun.) 14:00〜 ＠駒場キャンパス 東大TeX愛好会

## tcolorboxマニュアルって知ってる？

ascolorboxのあざらしさんが30回読んだあれです。現在tcolorboxパケージの最新は2019/11/15にリリースされたver 4.22です。あの頃よりも機能増えています。

[latest versionのマニュアルへのリンク](http://texdoc.net/texmf-dist/doc/latex/tcolorbox/tcolorbox.pdf)

ライブラリを読み込むことでちょっと凝った機能が使えるようになります。

- 10章，skins（ここを弄ってデザインするのがascolorboxの真髄だったわけですね）
- 14章，vignette（額縁風）
- 15章，***raster***（箱を並べる）
- 16章，***listings***，***listingsutf8***，***minted***（ソースコード装飾）
- 17章，theorems（数学の「定理」を囲むのにちょっと便利）
- 18章，breakable（普段から使ってますよね，これができるのがtcolorboxやmdframedの強み）
- 19章，***magazine***（breakableの進化系。箱の中身を保存しておいて配列として呼び出せる。）
- 20章，***poster***（箱の自由な配置）
- 21章，fitting（箱の中身をなんとしてでも詰め込む）
- 22章，hooks（箱の最初に記述する内容と最後に記述する内容を固定する）
- 23章，xparse（` ⧵DeclareTColorBox{ascolorbox●●}{O{red} m d"" O{}}{......}`でおなじみ）
- 24章，external（図版として単独でコンパイル）
- 25章，documentation（パケージのdocumentationを書くのに便利そうなやつを一括読み込み）

我々的に大事＆面白いのはraster，ソースコード装飾，magazine，posterあたり。今日はこの辺を扱っていきます。機能の紹介です。実装に踏み込もうという会じゃないです今日は

### 事例集

- rasterライブラリ
  - 「五月祭の前売り券を作りたい！」
  - 「夏の学習計画を書き込ませるカレンダーを作りたい！」
- listingsutf8ライブラリ
  - TeX愛好会民なのでマクロを書くことが多い。ソースコードを公開した文章を作りたいなあ
  - TeX愛好会民なのでゼミ資料にTeX言語を書きたい。ソースコードを貼りたいなあ
  - 他の言語でプログラミングしているけど，ソースコードを貼り付けてドキュメントを作りたいなぁ。LaTeX組みがいいんだけど……
- magazineライブラリ
  - 回り込みしてくる図があってブツブツ枠囲みが分割されるんだよね
  - この枠囲みの続きは１ページ飛ばした先なんだよね
  - ６つ折りのリーフレットを作りたい！内容の出力の順番は左上から５６１２３４の順番！
  - 夢！新聞作りたい！！←実現する前に鉄門だより副編集長おわってもうた……
- posterライブラリ
  - 学会発表ポスターのあの「たくさん配置する感じ」
  - ビラ作りたい！
  - 謎解きみたいな「配置が大事な紙」を一枚入念に作り上げたい！

## まずカスタマイズのQuick Ref（マニュアルp.10そのまま）

**tcolorboxマニュアルv4.22 p.11の図（2. Quick Reference）がすごくわかりやすい！絶対スクショしてデスクトップの壁紙にする！**

## rasterを理解するためには……

**tcolorboxマニュアルv4.22 p.287の図（15.1 Concept of Rasters）がすごくわかりやすい！絶対スクショしてデスクトップの壁紙にする！**

そしてサンプルファイルを見てもらおう（名札.tex）。tcolorboxの枠の太さがデフォルトで0.5mmなのよ。それでrow skipとcolumn skipを-0.5mmにしています。

``` \begin{tcolorbox}[boxrule=3pt]```みたいにすれば3ptに変えられる（right/left/top/bottom rulesをまとめて変更するオプション）。

あと，全てのboxに同じオプションをきかせたいなら`⧵tcbset{boxrule=3pt}`みたいにするといい。

なお，tcbraster使っているなら` \begin{tcbraster}[...]`のオプションの中に書けば反映されるけどね。（名札2.tex）

前売り券みたいに通し番号つけたかったらカウンタを用意すればいい話だね

余白を調整してかっこいい表紙を作るサンプルがsomebody_hyoushi.tex。最近誰かにあげたやつ。minipageを使って，下の２つのboxが次のページに飛んでいかないようにしている（なくてもちゃんと長さを調整できれば大丈夫なんだけど，デザイン中にしょっちゅう次のページに飛ぶのがめんどくさい）。

鉄緑の`⧵余白設定`が使えればストレスフリーで余白設定できるけれど，通常のLaTeX式でやろうとするとgeometry.styを使わなければ結構なバッドノウハウ。



## 次はlistingsを理解しよう

### minted

mintedを使うとソースコードのシンタックスハイライトが豪華になる。Pygmentsのインストールが必要。

http://ftp.jaist.ac.jp/pub/CTAN/macros/latex/contrib/minted/minted.pdf

このホームページからmintedの説明書の最新版を入れよう

python3のインストールを済ませた上で，`$ pip3 install pygments`です。

普通に`⧵begin{minted}{C}`〜とかもいいね。でもそのままではunbreakableなのです。よってmdframedやtcolorboxの出番だということ。

#### mintedのトラブルシューティング

https://qiita.com/la_float/items/2884a4d80a54ffa89a34

- `[cache=false]`はつけよう。パッケージオプション。なお，minted単独で使うのではなく，tcolorboxのlistingsでmintedオプションを使うなら多分デフォルトで入ってるんだろうけどcache=falseしなくてだいじょうぶですねぇ

- ターミナルからコンパイルする分にはいいけど，TeXShopなどの統合環境を使う場合にはソフト上からpygmentsへのPATHを通す必要あり。PATHを通すのが面倒ならlatexコマンドの置き場所にシンボリックリンクを置いてしまえ。「パーソナルスクリプト」は今ptex2pdfにしていたので，それと同じ場所におけばいい。なるほどね。

```bash
$ which ptex2pdf 
		%-> /Applications/TeXLive/texlive/2017/bin/x86_64-darwin/ptex2pdf
$ which pygmentize 
		%-> /Users/MENDELEVIUM/anaconda3/bin/pygmentize
$ sudo ln -s /Users/MENDELEVIUM/anaconda3/bin/pygmentize /Applications/TeXLive/texlive/2017/bin/x86_64-darwin/pygmentize

```

なかなかいいぞ！

紹介しなかったやつ

- listing side text/side listingモード：左右/右左に分ける。outside text/outside listingモードもある。listing side comment, comment side listing, comment outside listing, listing above text, text above listing, listing above comment, comment above listing, などなど多彩だなあ……
- image comment, tcbimage comment：ファイルを指定して画像張り込み
- pdf comment：ファイルを指定してpdfの概観張り込み！
- run pdflatex：pdflatexでソースをコンパイルする。--shell-escape必要。同様にrun xelatex, run biber, run lualatex, run makeindex,などもうたくさんある

### listing, listingutf8

mintedほどシンタックスハイライトは豪華にならないが，それでもいいなら。--shell-escapeが嫌いなら。オプションの書き方がmintedと若干違うので注意。

listing, listingutf8の書式はtcolorboxmanualを見る。mintedの書式はmintedの説明書を見る。という違いもあるので注意！



## magazine はTeX愛好会ゴコロをくすぐるライブラリ！

boxの内容をarray（配列）として収容でき，とりだせるライブラリです。

＜arrayとは？＞ まあ，プログラミングやったことあるなら知ってるでしょうけど！

tcbrasterとmagazineの組み合わせで行くんだけど，これの設定の方法が難しい。

極力store to box array側にオプションたちを寄せる。tcbrasterはtcboxedrasterになっているようなイメージでオプションを書く（mentsuke.tex）

⧵useboxarrayでboxの中身を消費しない（copy相当），⧵consumeboxarrayでboxの中身を消費する（box相当）

※rorate=180したときに1/4paperheightだけ下にずれるのでその分を補正したりしている。



## posterで学会発表のポスター風！

かなり配置が自由。もちろん，rasterを使ってもできるんですけど（column数とrow数を100 or 1000くらいにして，multicolumnとmultirowの個数を調整することで擬似的に座標を実現），posterを使うとだいぶかきやすくなっていてストレス軽減になる！チュートリアルは

**tcolorbox-tutorial-poster.pdf**

なので注意！https://ctan.math.illinois.edu/macros/latex/contrib/tcolorbox/tcolorbox-tutorial-poster.pdf

上の例っぽいものを作ってみよう。チュートリアルにだいたい沿っていますが，Step by Stepでお示しします。

1. 枠線を引く

```latex
\documentclass[dvipdfmx,12pt]{article}\usepackage[a3paper,landscape]{geometry}\usepackage[poster]{tcolorbox}
\pagestyle{empty}
\begin{document}
\begin{tcbposter}[%
coverage = {spread},
poster = {showframe,columns=5,rows=4},
]
% Here, we insert the poster content later
\end{tcbposter}
\end{document}
```

2. 箱を配置する

```LaTeX
% Here, we insert the poster content laterのところに……
\posterbox{name=logo,column=1,span=1,below=top}{%
LOGO later
}
\posterbox{name=title,column=2,span=4,below=top}{%
\centering{\bfseries\Huge RESEARCH TITLE}\\[3mm]
HOGEHOGE, fugafuga, piyopiyo
}
\posterbox[adjusted title=Background]
{name=background,column=1,span=1,below=logo}{}
\posterbox[adjusted title=Hypothesis and Protocol]
{name=hypothesis,column=1,span=1,between=background and bottom}{}
\posterbox[adjusted title=Result6]
{name=result6,column=2,span=1.5,above=bottom}{}
\posterbox[adjusted title=COI]
{name=coi,column*=5,span=2.5,above=bottom}{}
\posterbox[adjusted title=Take Home Messages]
{name=takehome,column*=5,span=2.5,above=coi}{}
\posterbox[adjusted title=Result3]
{name=result3,column=2,span=1.5,above=result6}{}
\posterbox[adjusted title=Result4]
{name=result4,column*=4,span=1.5,above=takehome}{}
\posterbox[adjusted title=Result5]
{name=result5,column=5,span=1,above=takehome}{}
\posterbox[adjusted title=Result1]
{name=result1,column=2,span=2,between=title and result4}{}
\posterbox[adjusted title=Result2]
{name=result2,column=4,span=2,between=title and result4}{}
```

相対的位置で決めていきます。topとbottomは最初から定義されている。

3. 箱の装飾を行います。枠線はもういらないので除去（showframe=false）

```latex
\begin{tcbposter}[%
coverage = {spread},
poster = {showframe=false,columns=5,rows=4},
boxes={sharp corners,
colframe=white,
colbacktitle=red!40!black,
coltitle=white,
fonttitle=\sffamily\bfseries,
boxrule=0pt,
colback=white,
}
```

あとは中身を入れていく！

サンプルファイルをご覧ください。どうなってるかみやすいようにしてあるので，完成形を見るためには

showframe=falseにして（枠線を消す），opacityfill=0.5を消して（透明度をなくす）ください。

今回のファイルではrow=指定を使っていませんが，row指定を使うことでばっちり高さも揃えられます。

オプションの一覧はtcoloboxマニュアル本体の20章を是非。

