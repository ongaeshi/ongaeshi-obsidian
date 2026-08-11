---
title: デイリーノートの末尾にさっとメモを残せる Obsidian Daily Appender(oda) を作った - おんがえしの blog
source: https://ongaeshi.hatenablog.com/entry/obsidian-daily-appender
author:
  - "[[おんがえし]]"
published: 2026-08-03
created: 2026-08-12
description: "はじめに PCで作業中にふと思いついたことや、どのノートに記録するかをすぐに決められないことは Obsidian のデイリーノートの一番下に## つぶやきという見出しを作って、時刻と一緒に残すようにしています。 1週間に1回程度眺めるだけでも楽しいですし、ここから発展して新しいノートが作られることもよくあります。 ## つぶやき 07:00 起きた。 --- 12:55 reading タグの付いた記事を Gemini に翻訳してもらって読んでいくシステムを作る。 --- 12:57 #transrate みたいなタグを付けて翻訳したらタグ外すのがいいか。 --- 15:51 折りたたみキーボ…"
tags:
  - clippings
categories:
  - "[[読んでる]]"
---
## はじめに

PCで作業中にふと思いついたことや、どのノートに記録するかをすぐに決められないことは Obsidian のデイリーノートの一番下に `## つぶやき` という見出しを作って、時刻と一緒に残すようにしています。

1週間に1回程度眺めるだけでも楽しいですし、ここから発展して新しいノートが作られることもよくあります。

```
## つぶやき
07:00 起きた。 

---
12:55 reading タグの付いた記事を Gemini に翻訳してもらって読んでいくシステムを作る。 

---
12:57 #transrate みたいなタグを付けて翻訳したらタグ外すのがいいか。

---
15:51 折りたたみキーボードこれよさそう。 [MOBO Keyboard 2 | MOBO](https://mobo-jp.com/products/mobo-keyboard2/) #tlog

---
19:43 肩が痛い。
```

このシステムでしばらく運用していたのですが、何か書きたいことがあるたびに、

1. Obsidianを開くまたはアクティブにする。
2. 今日のデイリーノートを開く
3. 末尾に時刻とメモを残す

というのが、段々と面倒になってきて、そのうちやめてしまいました。それ以降はデイリーノートの1番上に書いたり、毎回新規ノートに書いたり色々と試していたのですがどれもしっくりとしない日々が続きました。

## oda （Obsidian Daily Appnder）

ある日 [Obsidian CLI](https://obsidian.md/ja/help/cli) の仕様を眺めていると、 `obsidian daily:append content=CONTENT` でデイリーノートの末尾にテキストを挿入できる機能があることを発見しました。これを使えばコマンドラインからデイリーノートにつぶやきするツールが作れそうです。

ソフトウエアの名前について考えてみます。 Obsidian Daily Appnder は正式名としてはよさそうですが、CLI に毎回入力するものとしては長い。頭文字を取ると oda、これはよさそうです。キーボード入力もそこまで難しくない。oda でいくことにしました。

## 仕様（プロンプト）の記述

```
obsidianのデイリーノートに連続で複数行入力できるTUIをC#を使って作成したい。

- ソフトウェア名は Obsidian Daily Appender
- コマンド名は oda.exe (Obsidian Daily Appender の略)
- oda で実行するとTUIが起動
- Enterで入力実行。Shift+Enterで複数行入力できる。
- 基本的な入出力の仕様は C:\Users\ongaeshi\Code\obsidian_tool\tweet.rb を参考にせよ。(obsidian CLI を中継して入力する形でOK)
```

[tweet.rb](https://github.com/ongaeshi/obsidian_tool/blob/main/tweet.rb) は oda を作る前に Ruby で作ったプロトタイプです。基本的な仕組みや入力文法はここで検証しています。

CLIから複数行入力するのにいくつかのプログラム言語とライブラリを調べていたのですが、不具合なく日本語入力で複数行入力できるライブラリを見つけることができなかったので、使い慣れたC#上で検証してみるとそれなり動く物が作れたので自作することにしました（第一弾としては Windows Terminal 上で動けばOKとする。）。TUIのライブラリとしては [Spectre.Console](https://spectreconsole.net/) を使っています。

後は Geimini とキャッチボールします。

## oda の完成

最終版はこんな感じになりました。手元で2ファイルです。

[ongaeshi/ObsidianDailyAppender](https://github.com/ongaeshi/ObsidianDailyAppender/) ([README](https://github.com/ongaeshi/ObsidianDailyAppender/blob/main/README.ja.md) | [Download](https://github.com/ongaeshi/ObsidianDailyAppender/releases))

![](https://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20260802/20260802110814.png)

odaを起動してテキストを入力してEnterするとリアルタイムに Obsidian に反映されます。 Shift+Enterで複数行入力が可能です。履歴は Alt+↑↓

![](https://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20260802/20260802110747.gif)

今まで思い付いたことをObsidianに書こうとしたときに、Obsidianを探したりノートを探すのに手間取る認知負荷が無駄に高かったのですが、このツールのおかげで大分楽になりました。似たようなことをモバイル上でもやっているのですがそれはまたそのうちに。