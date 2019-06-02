# マクロを定義してみよう！ （\def,\newcommand,\NewDocumentCommand）

2019/6/5(Wed.) 18:00〜 初級者ゼミ 19:00〜 中上級者ゼミ ＠駒場キャンパス 東大TeX愛好会

<!--- environmentについては扱いましょうかね・・・ --->

<!--- \long については後で出てくるので扱っていただきたいです。 --->



## 今までのマクロ定義の弱点（以下，Y.Miz）

<!---大浦さんへ：問題点などあれば書き換えてください<(_ _)>-->

ここまでは```\def```，```\newcommand```，```\DeclareRobustCommand```などを紹介したが・・・

- `\def`の問題点
  - そのマクロが定義済みか否かにかかわらず，命令を上書きしてしまう。（なんと`\def\def`のような命令も作成可能）
  - オプション引数なども作ることが出来るが，`\@ifnextchar`や`\@ifstar`などを使ってゴリゴリやる必要があり，多少面倒。

- `\newcommand`の問題点
  - オプション引数を１つしか，しかも１つめの引数しかとれない。
  ```
  \newcommand\マクロ名[引数の個数][オプション引数のデフォルト値]{マクロの定義}
  ```
  のように。
    - 例
    ```
    \newcommand\chikoku[3][8時30分]{#2くんが#3に着いたのは#1です。}
    ```
    ↓
    ```
    \chikoku[9時ちょうど]{太郎}{学校}
     -> 太郎くんが学校に着いたのは9時ちょうどです。
    ```
    ```
    \chikoku{次郎}{会社}
     -> 次郎くんが会社に着いたのは8時30分です。
    ```

→もっと自由度の高いマクロ定義がしたい！ということで・・・

## Xparseパッケージ



### 提供されているマクロ（環境は除く）
`\NewDocumentCommand`，`\RenewDocumentCommand`，`\ProvideDocumentCommand`，`\DeclareDocumentCommand`，`\NewExpandableDocumentCommand`，`\RenewExpandableDocumentCommand`，`\ProvideExpandableDocumentCommand`，`\DeclareExpandableDocumentCommand`

全てのマクロに関して書き方は同じ。
```
\NewDocumentCommand\マクロ名{引数指定}{マクロの定義}
```


### マクロごとの説明


- `\NewDocumentCommand`

  LaTeX標準でいう`\newcommand`と同じような働き。マクロが定義済みの時はエラーを吐く。

- `\RenewDocumentCommand`

  LaTeX標準でいう`\renewcommand`と同じような働き。つまり，定義済みのマクロを書き換える。

- `\ProvideDocumentCommand`

  LaTeX標準でいう`\providecommand`と同じような働き。つまり，定義しようとしたマクロが未定義の時のみ定義する。

- `\DeclareDocumentCommand`

  TeXでいう`\def`と同じような働き。つまり，定義しようとしたマクロがすでに定義済みか否かにかかわらず，上書きして書き換える。

- `ほにゃららExpandableDocumentCommand`

  上記のそれぞれに`Expandable`をつけたもの。そもそもxparseはLaTeX3チームによるexpl3言語の概念が入り込んでおり（xparse.styを読むと分かる）そのexpl3における「完全展開可能」なマクロを作るためのもの。難しいし自分でもはっきりと理解できていないので飛ばします。


### 引数の指定方法

ここがxparseのなかでも相当に使いやすい箇所です。xparseでは引数の個数などを指定するのでは無く，***引数一つ一つについて***，オプション引数か必須引数かなどを指定できる。

- 指定方法の例：
  ```
  \NewDocumentCommand\マクロ名{m O{hoge} o m+ r(] }{マクロの定義}
  ```
  この意味についてであるが，
  - 第１引数：必須引数。
  - 第２引数：オプション引数。デフォルトの値は`hoge`
  - 第３引数：オプション引数。デフォルトの値はなく，値が入っていない場合は`NoValue`を返す。
  - 第４引数：必須引数。且つ，この引数は長い引数で，改行を挟むことができる。
  - 第５引数：必須引数。ただし，引数は(から]までの中身

こういった，非常に自由度の高いマクロを定義することが出来る。この引数の指定方法について，以下で述べる。

- 必須引数
  - `m`…一般的な引数で，一つのトークンで無い場合は{}で囲まれます。  
  - `r<char1><char2>`…必須引数ですが，引数の範囲の開始が`<char1>`で，終了が`<char2>`になります。もし`<char1>`が見つからなければ`-NoValue-`マーカーが挿入され，エラーを吐きます
  - `R<char1><char2>{<default>}`…必須引数ですが，引数の範囲の開始が`<char1>`で，終了が`<char2>`になります。もし`<char1>`が見つからなければ`-NoValue-`マーカーのかわりに`{default}`が挿入され，エラーを吐きます。（rとの実用上の違いが分かりません。）
  - `v`…引数を**verbatim** なものとして扱います。（つまり，普段は特殊文字として扱われる文字も引数として扱えるようになる）引数は{}で囲うか，`%,\,#,{,},␣`以外の同じ文字で囲みます。
  - `b`…環境なので省略
- オプション引数
  - `o`…一般的なオプション引数で，[]で囲われます。省略された場合は`-NoValue-`を返します。
  - `d<char1><char2>`…必須引数の`m`と`r`のような関係がオプション引数で言う`o`と`d`で，必須引数で言う`r`のように，引数の開始と終了を`<char1>``<char2>`で指定可能です。省略された場合は`-NoValue-`を返します。
  - `O{<default>}`…`o`と同じですが，省略された場合は{}内のデフォルト値を返します。
  - `D<char1><char2>{default}`…`d`と同じですが，省略された場合は{}内のデフォルト値を返します。
  - `s`…star（`*`）の判定を行います。`*`が存在する場合は`\BooleanTrue`，存在しない場合は`\BooleanFalse`となります。
  - `t<char>`…`s`の，★以外を`<char>`によって指定出来るヴァージョンです。
  - `e{<char>}`…よく分かりません・・・
  - `E{<char>}{<default>}`…よく分かりません・・・

- その他
  - 引数に`+`を指定することで，長い引数の指定が可能になります。例えば，`m+`のように使います。
  - `>prosessor`に関してはまだよく分かりません・・・


### NoValue の際の条件分岐

xparseでは先ほど`NoValue`を返しうる場合，あるいは`\BooleanTrue``\BooleanFalse`を返しうる場合を紹介しましたが，その結果を基に条件分岐する`\IfValueT`，`\IfValueF`，`\IfValueTF`，`\IfNoValueT`，`\IfNoValueF`，`\IfNoValueTF`，`\IfBooleanT`，`\IfBooleanF`，`\IfBooleanTF`が用意されています。
以下のように使います。
```
\IfNoValueTF{<argument>}{<true code>}{<false code>}
```
もちろん，TやFのみの場合は，片方の場合のみの動作を指定します。
