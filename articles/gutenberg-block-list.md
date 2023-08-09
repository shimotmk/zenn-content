---
title: "【WordPress5.8】Gutenbergコアのブロック名、キーワード、スタイル一覧" # 記事のタイトル
emoji: "🧱" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["WordPress","gutenberg"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

WordPressのブロックエディターでブロックをカスタマイズするときにブロック名やキーワードのリストがあると記事を書くときや不要なブロックを非表示にしたい時などに便利です。

不要なブロックを指定するためにブロック名が必要になったので一覧にしてこれを機にまとめておくことにしました。

2022年1月にはWordPres5.9がリリースされ使えるブロックも増えるので変更があったらコメントお願いします！

最新情報が見つけやすいサイトを見つけたのでこのサイトでみても良いかもです
https://wphelpers.dev/blocks

## 確認方法

WordPressのブロックエディタを開いてコンソールで`wp.blocks.getBlockTypes()`もしくは`wp.blocks.getBlockVariations('core/embed')`を打つと使用できるブロック一覧が確認できます。

https://twitter.com/shimo_tmk/status/1468511921032867848

jsから実行したい場合
```js
wp.domReady(function () {
  // ブロックタイプ確認方法
  console.log(wp.blocks.getBlockTypes());

  // 埋め込みブロック確認方法
  console.log(wp.blocks.getBlockVariations('core/embed'));
});
```
テーマやプラグインでJavaScriptを読み込めばコンソールに表示されます。

### 検証環境
- Twenty Twenty
- 使用プラグインなし

※なぜTwenty Twentyで検証しているかは`register_block_style`を使用していないからです。

## 「テキスト」(text)カテゴリーのブロック

| title | name | keywords | styles |
| ---- | ---- | ---- | ---- |
| 段落 | core/paragraph | テキスト | ---- |
| 見出し | core/heading | タイトル, サブタイトル | ---- |
| リスト | core/list | 箇条書きリスト, 順序付きリスト, 番号付きリスト | ---- |
| 引用 | core/quote | 引用, 引用 | デフォルト,大サイズ |
| コード | core/code | ---- | ---- |
| カラム | core/column | ---- | ---- |
| クラシック | core/freeform | ---- | ---- |
| 非サポート | core/missing | ---- | ---- |
| 整形済みテキスト | core/preformatted | ---- | ---- |
| プルクオート | core/pullquote | ---- | デフォルト,単色 |
| テーブル | core/table | ---- | デフォルト,ストライプ |

## 「メディア」(media)カテゴリーのブロック

| title | name | keywords | styles |
| ---- | ---- | ---- | ---- |
| 画像 | core/image | img, 写真, 画像 | デフォルト,角丸 |
| ギャラリー | core/gallery | 画像, 写真 | ---- |
| 音声 | core/audio | 音楽, 音声, ポッドキャスト, 録音 | ---- |
| カバー | core/cover | ---- | ---- |
| ファイル | core/file | 文書, pdf, ダウンロード | ---- |
| メディアとテキスト | core/media-text | 画像, 動画 | ---- |
| 動画 | core/video | 動画 | ---- |

## ウィジェット(widgets)カテゴリーのブロック

| title | name | keywords | styles |
| ---- | ---- | ---- | ---- |
| ショートコード | core/shortcode | ---- | ---- |
| アーカイブ | core/archives | ---- | ---- |
| カレンダー | core/calendar | 投稿, アーカイブ | ---- |
| カテゴリー | core/categories | ---- | ---- |
| カスタム HTML | core/html | 埋め込み | ---- |
| 最新のコメント | core/latest-comments | 最新のコメント | ---- |
| 最新の投稿 | core/latest-posts | 最新の投稿 | ---- |
| 固定ページリスト | core/page-list | メニュー, ナビゲーション | ---- |
| RSS | core/rss | atom, フィード | ---- |
| 検索 | core/search | 検索 | ---- |
| ソーシャルアイコン | core/social-links | リンク | デフォルト,ロゴのみ,カプセル形 |
| ソーシャルアイコン | core/social-link | ---- | ---- |
| タグクラウド | core/tag-cloud | ---- | ---- |

## デザイン(design)カテゴリーのブロック

| title | name | keywords | styles |
| ---- | ---- | ---- | ---- |
| ボタン | core/button | リンク | 塗りつぶし,輪郭 |
| ボタン | core/buttons | リンク | ---- |
| カラム | core/columns | ---- | ---- |
| グループ | core/group | コンテナ, ラッパー, 行, セクション | ---- |
| 続き | core/more | 続きを読む | ---- |
| ページ区切り | core/nextpage | 次のページ, ページネーション | ---- |
| 区切り | core/separator | 水平線, hr, 区切り線 | デフォルト,幅広線,ドット |
| スペーサー | core/spacer | ---- | ---- |
| テキストカラム (非推奨) | core/text-columns | ---- | ---- |
| サイトロゴ | core/site-logo | ---- | デフォルト,角丸 |
| サイトのキャッチフレーズ | core/site-tagline | 説明 | ---- |
| サイトのタイトル | core/site-title | ---- | ---- |
| 投稿テンプレート | core/post-template | ---- | ---- |
| クエリータイトル | core/query-title | ---- | ---- |
| クエリーのページ送り | core/query-pagination | ---- | ---- |
| 次のページネーションのクエリー | core/query-pagination-next | ---- | ---- |
| ページネーション数のクエリー | core/query-pagination-numbers | ---- | ---- |
| 前のページネーションのクエリー | core/query-pagination-previous | ---- | ---- |
| 投稿タグ | core/post-terms | ---- | ---- |

## 再利用(reusable)カテゴリーのブロック

| title | name | keywords | styles |
| ---- | ---- | ---- | ---- |
| 再利用ブロック | core/block | ---- | ---- |

## テーマ(theme)カテゴリーのブロック

| title | name | keywords | styles |
| ---- | ---- | ---- | ---- |
| クエリーループ | core/query | ---- | ---- |
| 投稿タイトル | core/post-title | ---- | ---- |
| 投稿コンテンツ | core/post-content | ---- | ---- |
| 投稿日 | core/post-date | ---- | ---- |
| 投稿の抜粋 | core/post-excerpt | ---- | ---- |
| 投稿のアイキャッチ画像 | core/post-featured-image | ---- | ---- |
| ログイン / ログアウト | core/loginout | ログイン, ログアウト, フォーム | ---- |

## 埋め込み(embed)カテゴリーのブロック

| title | name | keywords | styles |
| ---- | ---- | ---- | ---- |
| 埋め込み | core/embed | ---- | ---- |

## 埋め込み(embed)カテゴリーのブロック
| title | name | keywords | 
| ---- | ---- | ---- |
| Facebook | facebook | ソーシャル |
| Instagram | instagram | 画像, ソーシャル |
| WordPress | wordpress | 投稿, ブログ |
| SoundCloud | soundcloud | 音楽, 音声 |
| Spotify | spotify |  音楽, 音声 |
| Flickr | flickr |  画像 |
| Vimeo | vimeo | 動画 |
| Animoto | animoto | ---- |
| Cloudup | cloudup | ---- |
| CollegeHumor | collegehumor | ---- |
| Crowdsignal | crowdsignal | polldaddy, アンケート |
| Dailymotion | dailymotion | 動画 |
| Imgur | imgur | ---- |
| Issuu | issuu | ---- |
| Kickstarter | kickstarter | ---- |
| Meetup.com | meetupcom | ---- |
| Mixcloud | mixcloud | 音楽, 音声 |
| Reddit | reddit | ---- |
| ReverbNation | reverbnation | ---- |
| Screencast | screencast | ---- |
| Scribd | scribd | ---- |
| Slideshare | slideshare | ---- |
| SmugMug | smugmug | ---- |
| Speaker Deck | speakerdeck | ---- |
| TikTok | tiktok | 動画 |
| TED | ted | ---- |
| Tumblr | tumblr | ソーシャル |
| VideoPress | videopress | 動画 |
| WordPress.tv | wordpresstv | ---- |
| Amazon Kindle | amazonkindle | ebook |

WordPress 5.9からWolfram Cloud URLが増えるみたい。。。
https://github.com/WordPress/gutenberg/pull/35656

情報が追いついていなかったらコメント,ご指摘お願いします！
