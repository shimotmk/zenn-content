---
title: "【PHP】array_mergeを連想配列ではうまく使えない問題" # 記事のタイトル
emoji: "🪤" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["PHP","WordPress"] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

2つの配列をマージして新しく配列を作ってくれる関数`array_merge`は再帰処理をしてくれないので連想配列ではうまく使えません。
そこで自作で関数を作ってみました。それについてメモです。

# 目的

２つの配列
```PHP
$args = array(
	'string' => 'a',
	'array_1'  => array(
		'array_1_1' => array(
			'array_key_1_1' => 'array_value_1_1',
			'array_key_1_2' => 'array_value_1_2',
		),
	),
);
$defaults = array(
	'string' => 'b',
	'array_1'  => array(
		'array_1_1' => array(
			'array_key_1_1' => 'array_value_defaults_1_1',
			'array_key_1_2' => 'array_value_defaults_1_2',
		),
		'array_2_1' => array(
			'array_key_2_1' => 'array_value_2_1',
			'array_key_2_2' => 'array_value_2_2',
		),
	),
);
```
をこのような配列にしたい。

```PHP
$correct = array(
	'string' => 'a',
	'array_1'  => array(
		'array_1_1' => array(
			'array_key_1_1' => 'array_value_1_1',
			'array_key_1_2' => 'array_value_1_2',
		),
		'array_2_1' => array(
			'array_key_2_1' => 'array_value_2_1',
			'array_key_2_2' => 'array_value_2_2',
		),
	),
)
```
要するに配列のキーが存在していたらそのまま、キーが存在しなかったら$defaultsの配列のキーを補いたいと言うことです。

# 自作した関数

結論から言うとこのような関数
```PHP
function my_array_merge( $args, $defaults = array() ) {
	$parsed_args = $args;
	foreach ( $defaults as $key => $value ) {
		if ( empty( $args[ $key ] ) ) {
			$parsed_args[ $key ] = $value;
		} elseif ( is_array( $value ) && is_array( $args[ $key ] ) ) {
			$parsed_args[ $key ] = my_array_merge( $args[ $key ], $value );
		}
	}
	return $parsed_args;
}
```

ちなみに`array_merge`はPHPのマニュアルには以下のように書いてあります

> 入力配列が同じキー文字列を有していた場合、そのキーに関する後に指定された値が、 前の値を上書きします。しかし、配列が同じ添字番号を有していても 値は追記されるため、このようなことは起きません。
https://www.php.net/manual/ja/function.array-merge.php

参考：
https://qiita.com/sanogemaru/items/6312e44b6b6ee49240f3
https://qiita.com/kazu56/items/6947a0e4830eb556d575

# 余談
WordPressのoption値とデフォルト値をマージする際に今までは`wp_parse_args`と言う関数を使っていたのですが連想配列の場合はうまくマージされません

よくよく調べると`wp_parse_args`は内部的には`array_merge`をしているだけでした
https://developer.wordpress.org/reference/functions/wp_parse_args/
(というかマージする用の関数ではない気がする)

なので`wp_parse_args`は配列をマージするためには使わないほうが良い気がします

ticketも見てみましたがコアは多分対応しないでしょう。
https://core.trac.wordpress.org/ticket/19888
