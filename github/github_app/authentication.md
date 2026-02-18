# Github アプリ 認証
## 概要
インストール アクセス トークンを生成したり、アプリを管理したりするために、GitHub App として認証できる。

## 参考資料
* Document
  * https://docs.github.com/ja/apps/creating-github-apps/authenticating-with-a-github-app/authenticating-as-a-github-app

## 利用のタイミング
* `GITHUB_TOKEN` の代わり
  * `GITHUB_TOKEN` が自身をトリガーにできない 
* API を使って、organization のリソースにアクセスするためのインストール アクセス トークンを生成
* 複数のアカウント全体についてアプリのインストールを一覧表示
* アプリのインストールを一時停止
