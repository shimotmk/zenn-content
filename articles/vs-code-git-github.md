---
title: "VSCodeでよく使うGit,Gitコマンド,Github" # 記事のタイトル
emoji: "🗒" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["Git","Github"] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## VSCodeでよく使うGit,Gitコマンド,Github

個人的にVSCodeでよく使うGit,Gitコマンドをまとめました。
「gitでソースコードを管理,コマンド操作する！」というとなんだか難しいことをしていると感じる人もいるかもしれませんが、そこまで難しくないです。
コマンドの単語を知るとわかりやすいと思うので簡単な和訳も添えておきます。

Gitについて基本的なことは知っている前提で書くのでメモ的な感じです🙏

### pull(引く/引っ張る)

#### コマンド
```sh
git pull origin master
```
リモートのmasterブランチをpullしてローカルのブランチを最新にします

#### VS Code 
![](/images/git-pull.png)
Gitを繋げているフォルダでVS Code開いて左下から３番目をクリックするだけ

### checkout(チェックアウト/出ていく)

#### コマンド
```sh
git checkout -b ブランチ名
```
-bは新しくブランチを作る
-bが無い時はブランチを移動

#### VS Code 
![](/images/git-checkout.png)
VS Code開いて左下から２番目をクリックしたらブランチ名を決めて移動したり無い時は新しく作る

### add(追加) 
#### コマンド
```sh
git add ファイル
```

#### VS Code 
![](/images/git-add.png)
VS Codeのプライマリーサイドバー(一番左の黒いところ)からソース管理をクリックして追加したいファイルのプラスマークをクリック

![](/images/git-select-add.png)
VS Codeでは選択した箇所のみステージに上げることも出来る。
ソースコードの一部だけを変えたい場合などに良く使う。

### commit(コミット/預ける/委ねる)
#### コマンド
```sh
git commit -m “コミットメッセージ”
```

#### VS Code 
![](/images/git-commit.png)
addした内容が良ければコミットメッセージを書いてコミットボタンを押します
VS Codeの場合1行目がコミットタイトル、改行して2行目から説明文にあたります。

未来の自分のために分かりやすい説明を入れておくと良いでしょう

### push(押す/押し進める)
#### コマンド
```sh
git push origin ブランチ名
```

#### VS Code 
![](/images/git-push.png)
左下から３番目をクリックするだけ

### stash(隠す/しまう/蓄える)
新規開発をしているときにレビューや緊急の修正など一時的に今の状態を記録しておきたい時に使う

#### コマンド
```sh
git stash -u
```

#### VS Code 
![](/images/git-stash.png)
ソース管理のタイトル右に出てくる３点リーダーからスタッシュ→未追跡ファイルを含むをクリック
stash用のタイトルを書いて一時的に回避出来る。

■スタッシュ内容を戻す方法
![](/images/git-apply-stash.png)
command + shift + pでコマンドパレットを開き「スタッシュを適用」をクリック。
タイトルを選んで適用する

## ほか、たまに使うコマンド

#### 特定のファイルを戻すコマンド
```sh
git checkout {戻したいログのID} {戻したいファイル名}
```
戻したいログのIDは大体Githubから探しています
![](/images/github-commit-id.png)

参考
https://zenn.dev/ktysk/articles/2c6f72721ba338

#### ローカルのブランチを削除

Githubならマージされたブランチは自動で削除できる設定があります。
https://docs.github.com/ja/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/managing-the-automatic-deletion-of-branches

しかし、ローカルは手動で削除しないと行けません。
ブランチ名が被った時などに名前を変えたり少し困るので、ローカルのブランチを削除したい時もあると思います。

```sh
git branch -d ブランチ名
```
VS Codeなら
command + shift + pでコマンドパレットを開き「ブランチ削除」をクリック。

#### 一つ前のコミット取り消し

一つ前のコミット取り消す
```sh
git reset --soft HEAD^
```
参考
https://qiita.com/shuntaro_tamura/items/06281261d893acf049ed

## おまけGithubについて

### Githubでの検索
Githubでログを探すとき、「このコードなんでこういう実装にしたんだろう」と思うこと多々あると思います。
(人間誰にでもミスはあるので原因を見つけてその人をどうこう思ったりしない。)
そういう時はGithubの検索をうまく使うと便利です。

対象のリポジトリーが決まっているならそのリポジトリーに行って左上の検索窓から検索したいワードやコードを入れるだけ。
![](/images/git-search.png)

その組織(organization)やGithub全体から調べることもできます。
ただ、Github全体から調べるとヒットする数が多すぎることもあるのでもう少し対象の言語や期間を絞りたい時があると思います。
そういう時は以下のgithub.com/search/advancedのURLにいくとかなり細かく検索できます。
https://github.com/search/advanced

### GithubでのPull/Issues

Githubにログインして上のツールバーからpull requestsをクリックすると自分の出したプルリク、レビューリクエストされているプルリクなどが一覧で見れます。
https://github.com/pulls
![](/images/github-pulls.png)

Issuesも同じ。
https://github.com/issues
![](/images/github-issues.png)

リポジトリーやorganizationを跨って一覧で見れるので便利ですね。

### Github notifications 通知

Github notificationsは正直迷っています。。。
一応参考までにGithubのpull requestsやIssuesのコメントやりとりの通知設定について共有しておくと以下の感じ。

それぞれの設定は設定画面のNotificationsから見れるはず。
https://github.com/settings/notifications

カスタムしている設定は画像の通りです。
![](/images/github-settings-notifications.png)

Watchingしているチームやリポジトリーのやりとりがあれば通知がくる感じです。
あまり関係のないissueやリポジトリはunsubscribeすることもできるので最初は徐々に絞っていくと良いかもです。
![](/images/github-unsubscribe.png)

ベルマークをクリックしてコメントなどを確認したらDoneを押して完了
![](/images/github-settings-notifications.png)

## 最後に。

Git,Githubは素晴らしい仕組み、サービスですね。
ログが全て残る、画像や短い動画も共有できる。ほとんど無料で利用可能。
私はGit,Githubが無いと生きていけない体になってしまいました。
本当に感謝しています

それでは👋

