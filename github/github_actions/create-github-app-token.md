# create-github-app-token

## 概要
Github Apps で作成された App の権限を持つトークンを利用する方法

## 参考資料
* https://github.com/actions/create-github-app-token
* https://zenn.dev/tmknom/articles/github-apps-token

## 利用方法
### 1. Github App の secret　を追加
`APP_ID`
![App ID](./images/app-id.png)

`PRIVATE_KEY`
![PRIVATE_KEY](images/private-key.png)

### 2. Github Actions
* `GITHUB_TOKEN` を利用していたものの変数を書き換える
```yaml
jobs:
  plan:
    runs-on: ubuntu-latest
    steps:
    - name: Create Github App
      uses: actions/create-github-app-token@29824e69f54612133e76f7eaac726eef6c875baf # v2.2.1
      id: app-token
      with:
        app-id: ${{ secrets.APP_ID }}
        private-key: ${{ secrets.PRIVATE_KEY }}

    - name: Run tfaction setup for google_cloud
        uses: suzuki-shunsuke/tfaction/setup@4562d910bbacecf384c8aeda25332284fcf05f38 # v1.20.1
        with:
        # github_token: ${{ secrets.GITHUB_TOKEN }} # GITHUB_TOKEN を利用する場合
          github_token: ${{ steps.app-token.outputs.token }}
```

## メモ
### クリーンアップ処理で、トークンが見つからないログが出力された件
1回目
```
Post job cleanup.
Warning: Token revocation failed: Not Found - https://docs.github.com/rest/apps/installations#revoke-an-installation-access-token
```

2回目
```
Post job cleanup.
Token revoked
```

* エラーじゃない
* Action の status は success