# shared-workflows

他リポジトリから呼び出すための再利用可能な GitHub Actions ワークフロー集です。

## セットアップ

クローン後、以下のコマンドを実行してください。

```sh
npm install
```

## ワークフロー一覧

### `actions-lints`

GitHub Actions ワークフローファイルの lint を行います。

| ツール | 用途 |
|---|---|
| [actionlint](https://github.com/rhysd/actionlint) | 構文・セマンティクスのチェック |
| [ghalint](https://github.com/suzuki-shunsuke/ghalint) | ポリシーチェック（バージョン固定・パーミッションスコープ等） |
| [zizmor](https://github.com/zizmorcore/zizmor) | セキュリティ脆弱性の静的解析 |

**呼び出し例:**

```yaml
jobs:
  actions-lints:
    uses: yaizuuuu/shared-workflows/.github/workflows/actions-lints.yml@main
    permissions:
      contents: read
```

### `secret-scan`

git 履歴全体に対してシークレットスキャンを行います。

| ツール | 用途 |
|---|---|
| [gitleaks](https://github.com/gitleaks/gitleaks) | 汎用シークレット検出 |
| [git-secrets](https://github.com/awslabs/git-secrets) | AWS 認証情報パターンのスキャン |

**呼び出し例:**

```yaml
jobs:
  secret-scan:
    uses: yaizuuuu/shared-workflows/.github/workflows/secret-scan.yml@main
    permissions:
      contents: read
```

### `conventional-commits`

PRタイトルと各コミットメッセージが Conventional Commits 1.0.0 に準拠しているか検証します。

| ツール | 用途 |
|---|---|
| [amannn/action-semantic-pull-request](https://github.com/amannn/action-semantic-pull-request) | PRタイトルのチェック |
| [wagoid/commitlint-github-action](https://github.com/wagoid/commitlint-github-action) | 各コミットメッセージのチェック |

**呼び出し例:**

```yaml
jobs:
  conventional-commits:
    uses: yaizuuuu/shared-workflows/.github/workflows/conventional-commits.yml@main
    permissions:
      contents: read
      pull-requests: read
```

### `create-release`

`v*` パターンのタグプッシュ時に GitHub Release を自動作成します。前回タグからの変更履歴は GitHub が自動生成します。

**呼び出し例（workflow_call 非対応 — タグプッシュのみ）:**

```yaml
# このワークフローはタグプッシュで自動トリガーされます。
# 手動で v* タグをプッシュしてください。
```
