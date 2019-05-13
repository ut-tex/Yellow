# LaTeXで作るレポート　はじめの１歩

2019/5/15(Wed.) 18:00〜 ＠駒場キャンパス 東大TeX愛好会 domperor

## LaTeXのインストールは済んでいますか

***おすすめ：『美文書作成入門』付属インストーラ***。書籍部で買いましょう！

![美文書の画像](1.jpg)

- Macの場合：[MacTeX](https://doratex.hatenablog.jp/entry/20190502/1556775026) を使っている人が多い印象。

  本当は TeX Live を```install-tl```した方がよいのだが……

- Windowsの場合：[**あべのりインストーラ**](https://www.ms.u-tokyo.ac.jp/~abenori/soft/abtexinst.html)が便利。

  ただこれでインストールされるのは TeX Live ではなく W32TeX なので TeX Live が必要なら TeX Live を```install-tl```しましょう

- Linuxの場合：頑張って TeX Live を```install-tl``` してください
- インストールが面倒な場合：**Overleaf**を使いましょう

## インストール済みの人は‥‥

せっかくなのでOverleafを使ってみよう！次の本を買うと詳しくなれる！スクリーンショットふんだんでわかりやすい！！

![寺田先生の新刊の画像](2.jpg)

[↑↑↑寺田先生の新刊ですよ！！宣伝！！↑↑↑](https://www.amazon.co.jp/改訂第7版-LaTeX2ε美文書作成入門-奥村-晴彦/dp/4774187054)

1. まずは[Overleaf](https://www.overleaf.com/)にアクセス

2. registerしてlog in

3. New Project -> Example Project を選択，適当にProject Nameを入れてCreate

4. 日本語を使うための初期設定をしましょう

   - 左上 Menu を押す -> Settings で Compliler を pdfLaTeX から LaTeX にする

   - ![New File Buttonの画像](3.png) Menu ボタンの下の New File から ```latexmkrc``` を作る（拡張子なし）

     ```latexmkrc
     $latex='uplatex';
     $bibtex='upbibtex';
     $dvipdf='dvipdfmx %O -o %D %S';
     $makeindex='mendex -U %O -o %D %S';
     $pdf_mode=3;
     ```

   - main.tex の一行目を次のように変更

     ```変更前
     \documentclass{article}
     ```

     ↓

     ```変更後
     \documentclass[dvipdfmx,uplatex]{jsarticle}
     ```

   - main.tex に日本語をいくつか入れて，Recompileしてみましょう。通るはずです。

5. 参考文献を増やしたいときは references.bib をいじります。

   普通，参考文献を増やした後，反映するためには BibTeX コンパイル -> LaTeXコンパイル２回 が必要なのですが，Overleaf では自動化されていて Recompile １回で反映されます！

