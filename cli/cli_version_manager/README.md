# CLI Version Manager
## 概要
プログラミング言語（Node.js, Ruby, Python等）の複数バージョンをシステム内で共存させ、プロジェクト毎に容易に切り替えられるツール

## 特徴
### 1. 言語特化型バージョン管理ツール

| ツール名 | 対象言語 | 特徴 |
| :--- | :--- | :--- |
| **nvm** | Node.js | Linux/macOS向けで、最も人気がある |
| **nvm-windows** | Node.js | Windowsでnvmを利用するためのツール |
| **nodenv** | Node.js | ディレクトリ単位で自動的にバージョンを切り替え可能 |
| **n** | Node.js | シンプルで使いやすい |
| **Volta** | Node.js | 高速。プロジェクトごとのバージョン固定に強力 |
| **fnm** | Node.js | Rust製。高速でWindows環境にも適している |
| **rbenv** | Ruby | Rubyのバージョン管理のデファクトスタンダード |
| **pyenv** | Python | Python用 |
| **tfenv** | Terraform | Terraform用 |

### 2. 複数言語対応バージョン管理ツール

| ツール名 | 特徴 |
| :--- | :--- |
| **asdf** | プラグイン形式で多くの言語・ツールに対応。`.tool-versions` で一括管理可能 |
| **aqua** | YAMLでCLIツールを宣言的に管理。セキュリティ（チェックサム検証）と導入の容易さが特徴 |
| **mise** | Rust製で非常に高速。asdf互換に加え、環境変数管理やタスク実行機能も備える |

### 3. 選定のポイント
* 特定言語のみ → nvm, fnm, Volta ...
* 複数言語を扱う → asdf, aqua, mise
* OS環境 → fnm(Windows), nvm-windows(Windows)
* シンプルさ重視 → n

## 所感
* チーム開発であれば、環境の固定は必須
* 複数言語対応の方が使い勝手がいい