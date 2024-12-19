---
title: "Gutenberg Block IDを作る方法" # 記事のタイトル
emoji: "🔐" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["WordPress","gutenberg"] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

ブロック固有のIDを作りたいことがあると思う

いろいろな方法を模索したがやはりGutenbergの仕様に沿った形にするしかない

※大変申し訳ないのですが様々なプラグインを含め調査しているのでそのプラグインのバグ報告みたいな形になってしまうかもしれないですがご容赦ください

まずコアのissuesからブロックで固有のIDつくろうぜというissuesはここにある
https://github.com/WordPress/gutenberg/pull/34750

前提知識としてブロックはそのページだけでなく様々な場所でレンダリングされる可能性がある
具体的には以下の通り
- サイドバーやフッターのウィジェットでブロックを使用した時
- ブロックテーマでヘッダーやテンプレートパーツで使用した時
- 再利用ブロックで使用した場合

これらの条件があるのでブロック固有のIDを作ることは複雑なことになっている
例えば、ブロックエディタ上では固有のIDだとしてもサイドバーとコンテンツエリアで被ることはない？とか
再利用ブロックではIDは同じにしたい？など

## ではコアではどうしているか？

コアではグループブロック