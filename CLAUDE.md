# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 概要

他リポジトリから `workflow_call` で呼び出すための、再利用可能な GitHub Actions ワークフローを管理するリポジトリ。

## ワークフロー

### `actions-lints.yml`

GitHub Actions ワークフローファイルを以下の3ツールで lint する:

- **actionlint** — ワークフロー構文・セマンティクスのチェック
- **ghalint** (v1.5.5) — アクションのバージョン固定やパーミッションスコープなどのポリシーチェック
- **zizmor** — GitHub Actions のセキュリティ脆弱性の静的解析

`.github/workflows/*.yml` を変更するPR、および `workflow_call` でトリガーされる。

### `secret-scan.yml`

git 履歴に対してシークレットスキャンを実施:

- **gitleaks** — 汎用シークレット検出
- **git-secrets** — AWS 認証情報パターンのスキャン

PRおよび `workflow_call` でトリガーされる。

### `conventional-commits.yml`

PRタイトルと各コミットメッセージが Conventional Commits 1.0.0 に準拠しているか検証する:

- **amannn/action-semantic-pull-request** — PRタイトルのチェック
- **wagoid/commitlint-github-action** — 各コミットメッセージのチェック（`commitlint.config.js` で `@commitlint/config-conventional` を使用）

PRおよび `workflow_call` でトリガーされる。

### `create-release.yml`

`v*` パターンのタグプッシュ時に GitHub Release を自動作成する:

- `gh release create --generate-notes` で前回タグからの変更を自動生成

`v*` タグプッシュでトリガーされる（`workflow_call` 非対応）。

## 規約

ワークフローを追加・修正する際は以下を守ること:

- `uses:` はすべてコミット SHA に固定し、バージョンをコメントで明記する（例: `# v6.0.2`）
- すべてのジョブに `permissions:` を明示的に設定する（最小権限の原則）
- すべてのジョブに `timeout-minutes:` を設定する
- checkout ステップには必ず `persist-credentials: false` を設定する
- ワークフロー最上位に `permissions: {}` を設定してデフォルトで全権限を拒否する
