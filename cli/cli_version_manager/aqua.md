# aqua
## 概要
suzuki-shunsuke氏が開発した、Go言語で開発された、YAMLファイルでCLIツールのバージョンを宣言的に管理できるCLIツールマネージャー

## 参考資料
* Web
  * https://aquaproj.github.io/
* Github
  * https://github.com/aquaproj/aqua
* Document
  * https://aquaproj.github.io/docs/
* Zenn
  * [aqua CLI Version Manager 入門](https://zenn.dev/shunsuke_suzuki/books/aqua-handbook/viewer/index)

## 特徴
### 1. バージョン固定
* `aqua.yaml` に記載されたツールのバージョンを管理

### 2. 環境共有
* `aqua.yaml` を共有することで、複数人やCI/CD環境で全く同じツール構成を再現可能

### 3. 自動インストール
* ツール実行時に `aqua.yaml` を元に自動でインストールするため、別途インストールコマンドが不要

### 4. Renovate対応
* `aqua.yaml` のバージョンを自動更新し、常に最新のツールを利用できる

## 導入方法
### 1. `aqua` のインストール
* homebrewが楽
 
### 2. `aqua.yaml` 生成
CLIバージョンを固定したいディレクトリで実行
``` shell
aqua init
```

> [!Note]
> ### コマンドを実行するディレクトリ選択方法 
> 現在の __ディレクトリ__ から __ルート ディレクトリ__ にかけて設定ファイルを探す。<br/>→ 実行しているディレクトリが優先され、`aqua.yaml` や `aqua/aqua.yaml` がなければ、一つずつ上のディレクトリを探索していく。<br/>
> 基本は __リポジトリルート__ で実行でいい。<br/>
> [Configuration file path](https://aquaproj.github.io/docs/tutorial/config-path)

> [!Tip] 
> 実行したいディレクトリで、`aqua` のディレクトリを作成し、そのディレクトリで実行してもいい。<br/>
>  他の名前では無理そう。

### 3. CLI のパッケージをインストールする
エディタで編集もできるが、コマンドが便利
``` yaml
aqua g -i <PACKAGE_NAME>@<VERSION>
# aqua g -i hashicorp/terraform@v1.14.0
```


> [!Tip] 
> ### ローカルで aqua を使って、version 管理したい時（MacOS）
> #### ① 管理したいコマンドのパスを通す場合
> 1. 固定するものを探す
> ``` shell
> which terraform
> ```
> * aqua が有効な場合:
>   * 出力例: /Users/itoryusei/.local/share/aquaproj-aqua/bin/terraform (aqua の管理下のパス)
> * aqua が無効な場合（Homebrew などが動いている）:
>   * 出力例: /opt/homebrew/bin/terraform
> 2. `.zshrc` や `.bashrc` でパスを通す
> ``` shell
> export PATH="${AQUA_ROOT_DIR:-${XDG_DATA_HOME:-$HOME/.local/share}/aquaproj-aqua}/bin:$PATH"
> ```
> 3. 完成
> * `aqua.yaml` の中に記述されている、packages のバージョンが利用可能に
> ``` shell
> terraform --version
> ```
> #### ② コマンドで実行する場合
> 1. `aqua/aqua.yaml` or `aqua.yaml` が含まれているディレクトリでコマンドを実行する
> ``` shell
> aqua exec -- terraform --version
> ```