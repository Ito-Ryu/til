# checkout

## 概要

## 参考資料

## 利用方法
### `.github/workflow/xxx.yaml`
```yaml
jobs:
  checkout: 
    runs-on: ubuntu-latest
    permission:
      contents: read # For checkout
    steps:
      - name: Checkout
        uses: actions/checkout@de0fac2e4500dabe0009e67214ff5f5447ce83dd # v6.0.2
```

## Checkout と Github Apps Token の生成の順番
### checkout が先の場合
```yaml
jobs:
  checkout:
    runs-on: ubuntu-latest
    permission:
      contents: read # For checkout
      # pull-requests: write : これ描くのが正解
    steps:
      - name: Checkout
        uses: actions/checkout@de0fac2e4500dabe0009e67214ff5f5447ce83dd # v6.0.2

      - name: Create Github App
        uses: actions/create-github-app-token@29824e69f54612133e76f7eaac726eef6c875baf # v2.2.1
        id: app_token
        with:
          app-id: ${{ secrets.APP_ID }}
          private-key: ${{ secrets.PRIVATE_KEY }}
          repositories: ${{ github.event.repository.name }}
          permission-contents: write
          permission-pull-requests: write
    
      - name: Run git command
        run: |
          echo "=== [1] Checking Git Remote (to see if token is embedded) ==="
          git remote -v

          echo -e "\n=== [2] Checking GitHub CLI Auth Status (Scopes) ==="
          gh auth status
          
          echo -e "\n=== [3] Checking Raw API Scopes (Most Accurate) ==="
          # GitHub API にアクセスし、ヘッダーのみを取得
          HEADERS=$(curl -s -I -H "Authorization: Bearer ${{ steps.app-token.outputs.token }}"https://api.github.com/)

          # x-oauth-scopes ヘッダーを抽出して表示
          SCOPES=$(echo "$HEADERS" | grep -i "x-oauth-scopes:" || echo "x-oauth-scopes: None or noreturned")

          # github-actions[bot] -> GITHUB TOKEN
          # <GITHUB_APP_NAME>[bot] -> Github Apps Token
          ACCEPTED_SCOPES=$(echo "$HEADERS" | grep -i "x-accepted-oauth-scopes:" || echo "x-accepted-oauth-scopes: None")

          echo "Token Scopes: $SCOPES"
          echo "Accepted Scopes: $ACCEPTED_SCOPES"

      - name: Post Comment on PR
        if: github.event_name == 'pull_request'
        env:
          GH_TOKEN: ${{ steps.app_token.outputs.token }}
        run: |
        gh pr comment ${{ github.event.pull_request.number }} --body "Github Apps token の認証でメッセージを記入しています。"
```

### checkout が後の場合
```yaml
jobs:
  checkout:
    runs-on: ubuntu-latest
    permission:
      contents: read # For checkout
    steps:
      - name: Create Github App
        uses: actions/create-github-app-token@29824e69f54612133e76f7eaac726eef6c875baf # v2.2.1
        id: app_token
        with:
          app-id: ${{ secrets.APP_ID }}
          private-key: ${{ secrets.PRIVATE_KEY }}
          repositories: ${{ github.event.repository.name }}
          permission-contents: write
          permission-pull-requests: write

      - name: Checkout
        uses: actions/checkout@de0fac2e4500dabe0009e67214ff5f5447ce83dd # v6.0.2
        with:
          token: ${{ steps.app-token.outputs.token }}
          
    
      - name: Run git command
        run: |
          echo "=== [1] Checking Git Remote (to see if token is embedded) ==="
          git remote -v

          echo -e "\n=== [2] Checking GitHub CLI Auth Status (Scopes) ==="
          gh auth status
          
          echo -e "\n=== [3] Checking Raw API Scopes (Most Accurate) ==="
          # GitHub API にアクセスし、ヘッダーのみを取得
          HEADERS=$(curl -s -I -H "Authorization: Bearer ${{ steps.app-token.outputs.token }}"https://api.github.com/)

          # x-oauth-scopes ヘッダーを抽出して表示
          SCOPES=$(echo "$HEADERS" | grep -i "x-oauth-scopes:" || echo "x-oauth-scopes: None or noreturned")

          # github-actions[bot] -> GITHUB TOKEN
          # <GITHUB_APP_NAME>[bot] -> Github Apps Token
          ACCEPTED_SCOPES=$(echo "$HEADERS" | grep -i "x-accepted-oauth-scopes:" || echo "x-accepted-oauth-scopes: None")

          echo "Token Scopes: $SCOPES"
          echo "Accepted Scopes: $ACCEPTED_SCOPES" 
        
      - name: Post Comment on PR
        if: github.event_name == 'pull_request'
        env:
          GH_TOKEN: ${{ steps.app_token.outputs.token }}
        run: |
        gh pr comment ${{ github.event.pull_request.number }} --body "Github Apps token の認証でメッセージを記入しています。"
```


