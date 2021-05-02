# もっと黄色い本輪読ゼミ 第１回（第１章）

2021/05/11(Tue.) 21:00〜 ＠Online 東大TeX愛好会

## 1.1 TeX，LaTeXとは何か

### 1.1.2 TeX とその仲間たち

- 組版システムとしてのTeXの種類

    - pTeX……和文対応など
    - JTeX……和文対応など，ただし今はあまり使われていない
    - e-TeX……右向きに対応など
    - pdfTeX……組版結果をPDFで出力するなど
    - Omega……Unicode対応など，様々な拡張
    - XeTeX……Unicode対応など

    ここに紹介されていないものでも，
    - upTeX……pTeXの内部Unicode版
    - LuaTeX……pdfTeXの１種，Luaを内部で使用可能

    などがある。

- プログラミング機能による機能拡張

    - plain TeX ……ごく基本的な文書作成機能を持つ。TeX作者のKnuth氏の作。
    - LaTeX ……Lampport氏によるマクロ処理系。広く用いられている。
    - pLaTeX ……LaTeXをpTeXに対応させたもの

    他にも，
    - ConTeXt

    などのマクロ処理系がある。

現在日本で広く使われているのは，pLaTeXのようである。（要出典）

- パッケージ

機能拡張プログラムがある，CTANなどに公開されているものも多い。

- TeXディストリビューション

様々なTeXおよびその周辺ソフトウェア。TeXLiveやW32TeX，ptetexなど。

### 1.1.3 $TEXMFディレクトリ

TDSのお話ですが，これだけやるのもむしろあれなのでパス

### 1.1.4 TeX自身で行うことと……

TeXは箱を並べる作業，PDFに変換したりするのはdviware。

- ソースファイルを作成
- TeXやLaTeXを用いて処理，dviを出力
- dviwareで処理（ただしpdfTeXは別物）

dviwareについても紹介。

- xdvi……dviを読む
- dvipdfmx……dviをpdfに変換

早速dviを用意したのですが，xdviが動かなかったです。皆さんの環境では動きますか？



## 1.2 LaTeXによる文書作成の仕組み

実際にやってみましょう。

- ソースファイルの作成

`sample1.tex` を用意しましたので，使ってみてください

- タイプセットしてみましょう。（自分の環境では，デフォルトがpdfTeXでした）
    - dvi
    - aux
    - log
    
    それぞれの中身を確認。

- dviを見てみる

### 1.2.2 PostScript化

### 1.2.3 PDF化

それぞれやってみましょう。

### 1.2.4 和文テキストを含む処理

`sample2.tex`です。


### 1.2.5 LaTeX文書の作成の支援

- TeXShopなど

## 1.3 エラー・警告が生じた場合への対処

### 1.3.2  エラーの例

- Undefined Control Sequence

    `sample3.tex`
- 警告

    `sample4.tex`
- overfull \hbox

    `sample5.tex`

- 入力待ち

    `sample6.tex`

- 文字コード

    `sample7.tex`

### 1.3.3  対処法

究極的には慣れていくのが大切だが，

- 問題の切り分け・最小化
- メッセージを読む（どこがエラーなのかは分かります）

## 演習問題（一緒にやりましょう）

