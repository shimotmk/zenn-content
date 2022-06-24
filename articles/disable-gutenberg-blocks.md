---
title: "Gutenberg使わないブロックを非表示にする方法３選" # 記事のタイトル
emoji: "🙅‍♂️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["WordPress","gutenberg"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

ブロックを非表示にする方法を調べていた時に感じたことのメモです

この記事ではコードを少し書いてブロックを隠す方法を書いていきますがノーコードでやりたい人は以下の記事を参考にどうぞ。
https://www.vektor-inc.co.jp/post/disable-gutenberg-blocks/

## ブロックマネージャーで設定

![](/images/block-maneger.png)

一番簡単でコアの人も非表示にしたいならこれを推奨していると思う

### デメリット

 - ウィジェット
 - サイトエディター
などではカスタム出来ない
多分これはいづれ直る(コアが直している)

多分関連↓
https://github.com/WordPress/gutenberg/issues/31965

ユーザーごとに変更したり権限を設定することは出来ない

option値のように初期値を変えたりは出来ない(たぶん)

### ブロックマネージャーソースコード該当箇所
https://github.com/WordPress/gutenberg/tree/33d84b036592a5bf31af05b7710f3b2b14163dc4/packages/edit-post/src/components/block-manager
 
## inserterをfalseにする

- コアの非推奨ブロックもこの方法を使っている
 
https://twitter.com/shimo_tmk/status/1516045020326887424

### デメリット
コピペや複製などすれば今後も使える
 
# PHPで非表示にする

```PHP
add_filter( 'allowed_block_types_all', 
  function ( $allowed_block_types, $block_editor_context ) {
    // 許可するブロックタイプ
    $allowed_block_types = array(
      'core/paragraph',
      'core/heading',
    );
    return $allowed_block_types;
  },
  10,
  2 
);
```
### デメリット
ブロックディレクトリーの検索などに不具合がある。
https://github.com/WordPress/gutenberg/issues/33954
 
# unregisterでブロックの登録をやめる
そもそもブロックの登録をやめる

コード
```js
// my-plugin.js
wp.domReady(function () {
  // オフにしたいブロックを指定する
  wp.blocks.unregisterBlockType('core/paragraph'); 
  wp.blocks.unregisterBlockType( 'core/heading' );
});
```

### デメリット
もし使用しているページがあったらブロックが表示されない
ブロックのエラーが起きる

ちなみにブロック名の確認方法はこちらの記事に詳しく書いてありますが、ブロックエディターのコンソールで`wp.blocks.getBlockTypes() `と打てば出てきます。
https://zenn.dev/shimomura/articles/gutenberg-block-list
 
個人的にブロックを隠したいときはinserterをfalseで良いのかなと思います
もし使いたくなったら戻せるので。
