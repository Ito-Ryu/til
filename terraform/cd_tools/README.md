# Terraform CD Tools
## 概要
Terraformの実行（Plan/Apply）を自動化し、CI/CDパイプラインやプルリクエストベースのワークフローを実現するためのツール。チーム開発におけるState管理、レビュープロセス、ガバナンス強化を支援する。

## 特徴
### 代表的なTerraform CDツール

| ツール名 | タイプ | 特徴 |
| :--- | :--- | :--- |
| **Terraform Cloud** | SaaS | HashiCorp公式。State管理、Remote Execution、Private Registryなどの機能を提供。無料枠あり。 |
| **Atlantis** | OSS (Self-hosted) | プルリクエストのコメント（`atlantis plan` / `atlantis apply`）で操作するGitOpsツール。 |
| **Spacelift** | SaaS / Self-hosted | 高機能なIaC管理プラットフォーム。ポリシー管理（OPA）、ドリフト検出、依存関係の可視化などが強力。 |
| **GitHub Actions (tfaction)** | CI/CD (Workflow) | GitHub Actions上で動作。tfactionを使用することで、推奨されるベストプラクティス（Plan結果のPRコメント通知、Applyの自動化など）を容易に構築可能。 |

### 選定のポイント
* **手軽に始めたい / 公式サポート重視**: Terraform Cloud
* **コストを抑えたい / 自前でホストしたい / PRベースの操作**: Atlantis
* **大規模組織 / ガバナンス強化 / 高度な機能が必要**: Spacelift
* **GitHub中心のエコシステム / カスタマイズ性重視**: GitHub Actions (tfaction)

## 所感
* GitHub Actions (tfaction) はGitHubとの親和性が高く、既存のWorkflowに組み込みやすい。
* Atlantisはシンプルだが、セルフホストの運用負荷を考慮する必要がある。
* Terraform CloudはState管理の手間が省けるのが最大のメリット。
