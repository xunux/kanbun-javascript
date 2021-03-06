# 漢文訓読JavaScript

## 概要

漢文訓読JavaScriptは、青空文庫形式で記述した漢文データを「原文」、「訓読」、「読み下し文」などにHTMLで変換するライブラリです。

ブラウザでも、またはRhinoなどローカル環境でも動作します。

## 漢文の訓点の例

以下の漢文で例を示します。

    吾嘗終日食、終夜不寝、以思。

訓点を[青空文庫の注記記法](http://kumihan.aozora.gr.jp/kunten.html)で入力します。

    吾嘗終日不［＃レ］食、終夜不［＃レ］寝、以思。

送り仮名はカタカナ、ひらがなで*漢字と訓点の間に*入れます。

    吾嘗テ終日不［＃レ］食ラハ、終夜不［＃レ］寝ネ、以テ思フ。

青空文庫形式では、漢文の送り仮名は［＃（XX）］という形式で記入することになっていますが、カタカナ・ひらがなの場合は不要です。
（万葉仮名を用いる場合は、［＃（XX）］形式も利用可能です。）

必要に応じて、読み仮名を「青空文庫形式」で挿入します。

    吾《われ》嘗《かつ》テ終《しゅう》日《じつ》不《ず》［＃レ］食《く》ラハ、
    終夜《しゅうや》不《ず》［＃レ］寝《い》ネ、以《もつ》テ思《おも》フ。

このデータを、"kanbun"クラスのタグに入れると、JavaScript関数の処理対象となります。HTMLタグにidを設定し、個別に制御可能になります。

    <p class="kanbun">吾《われ》嘗《かつ》テ終《しゅう》日《じつ》不《ず》［＃レ］食《く》ラハ、終夜《しゅうや》不《ず》［＃レ］寝《い》ネ、以《もつ》テ思《おも》フ。</p>

## 利用方法

HTMLで利用する場合は、jqueryが必要です。

    <script type='text/javascript' src='./js/jquery.js'></script>
    <script type='text/javascript' src='./js/kanbun.js'></script>

以下の関数を呼び出すことで、漢文を整形します。IDを空文字列にすれば、全てのkanbunクラスのタグが処理対象になります。

- kanbun_html_to_original("#ID") … 入力テキストを表示します。
- kanbun_html_to_kanbun("#ID",bool,bool,bool,bool,bool) … 漢文を表示します。
- kanbun_html_to_kakikudashi("#ID",bool,bool,bool) … 書き下し文を表示します。

関数の詳細については、付属の [jsdoc](./doc/index.html) を確認ください。

## 漢文の例

再読文は、『【漢字】《【よみ】》【送り仮名】〈【再読よみ】〉【再読送り仮名】』で表現します。

- 原データ： 引キテ［＃レ］酒ヲ且《》二〈ス〉［＃レ］飲マント［＃レ］之ヲ。
    * 漢文：引酒且飲之。
    * 書き下し：酒ヲ引キテ且二之ヲ飲マントス。

- 原データ： 孤之〈ノ〉有ルハ［＃二］孔明［＃一］、猶《な》ホ〈ゴト〉シ［＃二］魚之〈ノ〉有ルガ［＃一レ］水。
    * 漢文： 孤之有孔明、猶魚之有水。
    * 書き下し： 孤ノ孔明有ルハ、猶《な》ホ魚ノ水有ルガゴトシ。

置き字は、その漢字の後ろに空の〈〉を付します。

- 原データ：青ハ取リテ［＃二］之ヲ於〈〉藍ヨリ［＃一］而〈〉青シ［＃二］於〈〉藍ヨリモ［＃一］
    * 漢文： 青取之於藍而青於藍
    * 書き下し：青ハ之ヲ藍ヨリ取リテ藍ヨリモ青シ

対応する訓点には、上中下・甲乙丙丁・天地人があります。

- 原データ： 使〈シ〉メヨ［＃人］籍《せき》ヲシテ誠《まこと》ニ不〈〉〈ズ〉［＃乙］以《もつ》テ［＃下］蓄《やしな》ヒ［＃二］妻子ヲ［＃一］憂《うれ》フルヲ［＃中］飢寒《きかん》ヲ［＃上］乱サ［＃甲レ］心ヲ、有リテ［＃レ］銭《ぜに》以《もつ》テ済《な》サ［＃地］医薬ヲ［＃天］。
    * 漢文：使籍誠不以蓄妻子憂飢寒乱心有銭以済医薬。
    * 書き下し： 籍《せき》ヲシテ誠《まこと》ニ妻子ヲ蓄《やしな》ヒ飢寒《きかん》ヲ憂《うれ》フルヲ以《もつ》テ心ヲ乱サズ、銭《ぜに》有リテ以《もつ》テ医薬ヲ済《な》サシメヨ。


## HTMLの例

付属の kanbun.html を参照ください。

## 漢文の書式

漢文は以下の書式で記述します。改行などの特殊記号は使えないのでご注意ください。

     漢文       := 漢文単位+
     漢文単位   := 漢字単位 読み仮名? 送り仮名* 再読仮名? 再読送り* 竪点? 訓点?
     漢字単位   := 漢字 異体字選択? | 句読点
     漢字       := "[㐀-\9fff]|[豈-\faff]|[\ud840-\ud87f][\udc00-\udfff]"
     句読点     := "[。、]"
     異体字選択 := "\udb40[\udd00-\uddef]" （異体字選択子：U+E0100...U+E01EF）
     読み仮名   := 表示読み | 非表示読み
     表示読み   := "《" 仮名漢字* "》"
          ※ 表示読みは、漢文・書き下し文の両方で漢字の脇に読みを表示する。
     仮名漢字   := 仮名 | 漢字
     仮名       := [ぁ-ヿ]
     非表示読み := "〈" 仮名漢字* "〉"
          ※ 非表示読みは、漢文では読みを表示せず、書き下し文では読みのみ表示する。
          ※ 置き字は非表示読みを空の〈〉として表現する。
     送り仮名   := 仮名+ | "［＃（" 仮名漢字+ "）］"
          ※ 万葉仮名がある場合は、後者を使用する。
     再読仮名   := 表示読み | 非表示読み
     再読送り   := 仮名+ | "［＃（" 仮名漢字+ "）］"
          ※ 万葉仮名がある場合は、後者を使用する。
     竪点       := "‐" ※ （仮定）竪点は２つ連続しない。
     訓点       := "［＃" 訓点文字 "］"
     訓点文字   := 順序点 | "[一上天甲]?レ"
     順序点     := [一二三四上中下天地人甲乙丙丁]

