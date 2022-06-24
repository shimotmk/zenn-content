---
title: "register_settingのdefaultとpropertiesをいい感じにする関数を作った" # 記事のタイトル
emoji: "⚙️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["WordPress","gutenberg"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

WordPressの管理画面をGutenbergコンポーネントで作ることにハマっています😅

オプション値を保存するためにrest apiを使えるようにしなくては保存できないのですが
register_settingのdefaultとpropertiesの設定が面倒だったので関数を作りました。
それについてメモです。

register_settingの公式リファレンス
https://developer.wordpress.org/reference/functions/register_setting/

register_settingのdefaultとpropertiesは手書きで書くとこのようにしなくてはいけません
```PHP
register_setting(
    'my_plugin_settings',
    'myPluginOptions',
    array(
        'type'         => 'object',
        'default'      => array(
            'some_str' => 'A',
            'some_int' => 3,
        ),
        'show_in_rest' => array(
            'schema' => array(
                'type'       => 'object',
                'properties' => array(
                    'some_str' => array(
                        'type' => 'string',
                    ),
                    'some_int' => array(
                        'type' => 'integer',
                    ),
                ),
            ),
        ),
    )
);
```
引用:
https://developer.wordpress.org/reference/functions/register_setting/#comment-5289

ちょっと見にくいしオプションが多くなったらなおさら見にくくなることが想定されます

## 個人的にいい感じのコード
 
```PHP
/**
 * Get defaults
 *
 * @param array $schema option schema.
 *
 * @return options
 */
function my_plugin_get_defaults( $schema ) {
	$default = array();
	foreach ( $schema as $key => $value ) {
		$default[ $key ] = 'object' === $value['type'] ? my_plugin_get_defaults( $value['items'] ) : $value['default'];
	}
	return $default;
}

/**
 * Get properties
 *
 * @param array $schema option schema.
 *
 * @return options
 */
function my_plugin_get_properties( $schema ) {
	$properties = array();
	foreach ( $schema as $key => $value ) {
		$properties[ $key ]['properties'] = 'object' === $value['type'] ? my_plugin_get_properties( $value['items'] ) : $value['type'];
	}
	return $properties;
}

/**
 * 設定項目の登録
 */
add_action(
	'init',
	function () {
		$option_schema    = array(
			'someString'         => array(
				'type'    => 'string',
				'default' => 'defaultString',
			),
			'someBool'         => array(
				'type'    => 'boolean',
				'default' => true,
			),
			'someInteger'         => array(
				'type'    => 'integer',
				'default' => 1,
			),
			'someNumber'         => array(
				'type'    => 'number',
				'default' => 1,
			),
			'someArray'         => array(
				'type'    => 'array',
				'default' => array(
					'array1',
					'array2',
					'array3',
				),
			),
			'someObject'          => array(
				'type'  => 'object',
				'items' => array(
					'someObjectItemsOne' => array(
						'type'  => 'object',
						'items' => array(
							'someBool'         => array(
								'type'    => 'boolean',
								'default' => true,
							),
							'someString'         => array(
								'type'    => 'string',
								'default' => 'defaultString',
							),
						),
					),
					'someObjectItemsTwo' => array(
						'type'  => 'object',
						'items' => array(
							'someBool'         => array(
								'type'    => 'boolean',
								'default' => true,
							),
							'someString'         => array(
								'type'    => 'string',
								'default' => 'defaultString',
							),
						),
					),
				),
			),
		);

		register_setting(
			'my_plugin_settings',
			'myPluginOptions',
			array(
				'type'         => 'object',
				'default'      => my_plugin_get_defaults($option_schema),
				'show_in_rest' => array(
					'schema' => array(
						'type'       => 'object',
						'properties' => my_plugin_get_properties($option_schema),
					),
				),
			)
		);
	}
);
```
register_settingのschemaがズレるとオプションが保存されないので関数などにしておくと便利

defaultやpropertiesの取得方法がオブジェクトの場合、無限に入れ子になる可能性があるので再帰関数を使いました。

https://qiita.com/7968/items/a8eec7a32f7e8a7c0bab

`$option_schema`はクラス化するなりグローバル変数にするなりして使う

あとはwordpress/apiやregister_rest_routeで保存処理を書けばOK
