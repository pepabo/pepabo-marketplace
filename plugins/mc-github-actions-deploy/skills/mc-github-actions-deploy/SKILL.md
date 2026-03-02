---
name: mc-github-actions-deploy
description: GitHub Actionsからロリポップ！マネージドクラウド(mc.lolipop.jp)への自動デプロイワークフローを生成する。「マネクラにCIデプロイ」「GitHub ActionsでMCにデプロイ」「マネクラの自動デプロイ」で自動適用
allowed-tools: Read, Write, Edit, Glob, Grep
---

# GitHub Actions → マネージドクラウド 自動デプロイ

GitHub リポジトリへの push をトリガーに、ロリポップ！マネージドクラウド（https://mc.lolipop.jp/）へ自動デプロイする GitHub Actions ワークフローを生成する。

## 前提条件

- マネージドクラウドにプロジェクトが作成済みであること
- プロジェクトの SSH 接続情報（ユーザー名）がわかっていること
- SSH 公開鍵がマネクラに登録済みであること

## ヒアリング

スキル実行時に AskUserQuestion で以下を確認する：

1. **マネクラの SSH ユーザー名は？**（例: `steep-night-8689`）
   - プロジェクト詳細画面の SSH/SFTP に表示されている
2. **デプロイ対象ブランチは？**（デフォルト: `main`）
3. **ワークフローの配置先は？**（デフォルト: `.github/workflows/deploy.yml`）

## デプロイの仕組み

マネクラは専用の Git リモートリポジトリを持ち、`master` ブランチに push するとビルド＆デプロイが実行される。GitHub Actions からこの Git リモートに push することで自動デプロイを実現する。

参考: https://note.com/mclolipopjp/n/n0e8dace1404b

## ワークフローテンプレート

```yaml
name: Deploy to Managed Cloud

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - uses: webfactory/ssh-agent@v0.9.1
        with:
          ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}

      - name: Add known hosts
        run: ssh-keyscan -H ssh.mc.lolipop.jp >> ~/.ssh/known_hosts

      - name: Add git remote
        run: git remote add lolipop ssh://<SSH_USER>@ssh.mc.lolipop.jp:22/

      - name: Deploy
        run: git push lolipop HEAD:master
```

### ステップの解説

| ステップ | 説明 |
|---------|------|
| `actions/checkout` | `fetch-depth: 0` で全履歴を取得（マネクラへの push に必要） |
| `webfactory/ssh-agent` | SSH 秘密鍵を ssh-agent に読み込む。鍵はメモリのみに保持されディスクに残らない |
| `Add known hosts` | `ssh.mc.lolipop.jp` のホスト鍵を登録し MITM を防ぐ |
| `Add git remote` | マネクラの Git リモートを追加 |
| `Deploy` | `HEAD:master` でマネクラの master ブランチに push（マネクラは master のみ受付） |

## 必要な Secrets

リポジトリの Settings > Secrets and variables > Actions から登録する。

| Secret 名 | 内容 | 取得方法 |
|-----------|------|----------|
| `SSH_PRIVATE_KEY` | SSH 秘密鍵 | マネクラに登録している鍵ペアの秘密鍵。`-----BEGIN OPENSSH PRIVATE KEY-----` から `-----END OPENSSH PRIVATE KEY-----` まで全文 |

### SSH 鍵を新規作成する場合

```bash
ssh-keygen -t ed25519 -a 100 -f mc-deploy-key -N "" -C "github-actions-deploy"
```

- `mc-deploy-key.pub` → マネクラの SSH 公開鍵設定に登録
- `mc-deploy-key` の中身 → GitHub リポジトリの Secrets に `SSH_PRIVATE_KEY` として登録
- 鍵ファイルは登録後にローカルから削除してよい

## 生成ルール

1. `<SSH_USER>` をヒアリングしたユーザー名に置換する
2. デプロイ対象ブランチが `main` 以外なら `on.push.branches` を変更する
3. ワークフローファイルを指定パスに Write ツールで書き出す
4. `.github/workflows/` ディレクトリが存在しない場合は作成する

## トラブルシューティング

| 問題 | 原因 | 解決策 |
|------|------|--------|
| `Permission denied (publickey)` | Secrets の秘密鍵が間違っている or マネクラに公開鍵未登録 | 鍵ペアの対応を確認 |
| `src refspec master does not match any` | リポジトリにコミットがない | 最低1つコミットしてから実行 |
| `Host key verification failed` | known_hosts への追加が失敗 | `ssh-keyscan` の出力を確認 |
| `failed to push some refs` | マネクラ側に既存の履歴がありコンフリクト | 初回は `git push lolipop HEAD:master --force` を検討 |
