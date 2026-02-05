# tfaction
suzuki-shunsuke氏が開発した、GitHub Actions上で高機能なTerraformのCI/CD（計画・適用・ドリフト検出）を容易に実現するフレームワーク。

[tfaction](https://github.com/suzuki-shunsuke/tfaction)

[tfaction docs](https://suzuki-shunsuke.github.io/tfaction/docs/)

## メリット
### GitHub Actionsによる運用
* 専用サーバーが不要で、GitHub ActionsだけでAtlantisのようなワークフローを構築可能

### モノレポ対応
* モノレポ（一つのリポジトリに複数の機能）環境でも、Plan / Apply が可能

### 便利な機能
* PRコメント: tfcmtを利用し、計画結果をPRに綺麗にコメント
* ドリフト検出: 定期的に実環境とコードの差分を検出し、レポート
* セキュリティ・品質管理: TrivyやTFLint、Conftestなどのチェックをパイプラインに組み込める

## 使い方
### Provider の設定
>If you use AWS, Configure AWS Identity Provider </br>
If you use Google Cloud, setup GCP Workload Identity Federation </br>

### 必須ファイル追加
* tfaction-root.yaml
* tfaction.yaml
* aqua.yaml

[tfaction setup](https://suzuki-shunsuke.github.io/tfaction/docs/config/setup)

[tfaction-example](https://github.com/suzuki-shunsuke/tfaction-example)

---

#  tfactoin-root.yaml
## 

