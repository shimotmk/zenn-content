---
title: "WordPressテーマにtheme.jsonを入れるとsupportsLayoutがtrueになってしまう問題について" # 記事のタイトル
emoji: "🃏" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["WordPress","gutenberg"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

theme.jsonを入れるとインナーブロックで全幅、幅広が選択出来ないことをなんとかしろと言われたことがあったのでtheme.jsonについていろいろ調べました。
それについてメモです。

## 結論

以下のようにするとtheme.jsonを使用しているテーマでもインナーブロックで全幅、幅広が設定できた。
 
```PHP
add_filter( 'block_editor_settings_all', __NAMESPACE__ . '\block_editor_settings_all' );
function block_editor_settings_all( $editor_settings ) {
	$editor_settings['supportsLayout'] = false;
	return $editor_settings;
}
```

## コアのコード

theme.jsonがあるかどうか判断している関数
https://github.com/WordPress/wordpress-develop/blob/ed1f411c564f79d003de8babb485884d0a71caa2/src/wp-includes/class-wp-theme-json-resolver.php#L398-L415

supportsLayoutを設定している変数$editor_settings
https://github.com/WordPress/wordpress-develop/blob/ed1f411c564f79d003de8babb485884d0a71caa2/src/wp-admin/edit-form-blocks.php#L201

最終的に$editor_settingsをjsonに変換してwp_add_inline_scriptで読み込んでいる
https://github.com/WordPress/wordpress-develop/blob/ed1f411c564f79d003de8babb485884d0a71caa2/src/wp-admin/edit-form-blocks.php#L277-L296

`$editor_settings`を整形する前の関数`get_block_editor_settings`でフィルターフックがある
https://github.com/WordPress/wordpress-develop/blob/ed1f411c564f79d003de8babb485884d0a71caa2/src/wp-includes/block-editor.php#L300-L423

## コアの考え方 全幅 (Full width) ≠ 画面幅
Full widthの場合はmax-widthがnoneになる
ブラウザ幅全体というわけではなく親要素の幅いっぱい
 
### コアのissues
[WordPress/gutenberg#29335](https://github.com/WordPress/gutenberg/pull/29335)
[WordPress/gutenberg#33374](https://github.com/WordPress/gutenberg/issues/33374)
 
おそらくGutenbergのエディターとフロントエンドのDOMが差が出てくることでWYSWYG(=見たままが得られる)ではないことが懸念されているのではないかと思う。
 
theme.jsonを入れるとthemeSupportsLayoutの条件に合致しているので編集画面でdivがwrapされない
https://github.com/WordPress/gutenberg/blob/e12ac149a1ec47518b011dd5d6d26e224429b1cb/packages/block-editor/src/components/block-list/block.js#L131-L153
https://github.com/WordPress/gutenberg/pull/38613
また左右のmargin設定を付けたときにどのようにするかなどの問題が出てくる

なので幅の設定を無効化しているのかなと思う。

*** またカバーブロック内で全幅にしたければインナーブロックをグループ化すれば全幅は使える ***
なぜ幅広が使えないかわからない。

## 他、参考になったもの
幅については以下も参考になりました。
https://snow-monkey.2inc.org/2020/01/06/snow-monkey-v9/#co-index-6
https://wordpress.tv/2019/12/21/toru-miki-things-to-be-learn-for-theme-developers/
https://speakerdeck.com/waviaei/gutenberg-yi-jiang-falsetemazuo-cheng-nixiang-kete-jin-xue-bubekikoto?slide=71

SWELLのリポジトリーでhamanoさんのコメントがあり
https://github.com/loos/SWELL/issues/239#issuecomment-1128692201
こんな感じで書くとjsからsupportsLayout:falseに無理くりではあるが出来ました。
ただ、編集画面が表示されたときには全幅、幅広は指定することは出来ず、何かしらのブロックを挿入すると幅の設定が出来るようになったりする。
 
個人的には現時点(2022/5/25)ではクラシックテーマにtheme.jsonを入れるのはババ抜きな気がする。。
 
