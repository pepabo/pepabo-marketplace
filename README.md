# Pepabo Marketplace

GMOペパボに関するClaude Codeプラグインを集めたマーケットプレイスです。

## インストール

### 1. マーケットプレイスを追加

Claude Codeで以下を実行:

```
/plugin marketplace add pepabo/pepabo-marketplace
```

### 2. プラグインをインストール

```
/plugin install pepabo@pepabo-marketplace
/plugin install suzuri-api@pepabo-marketplace
```

MCP連携:

```
/plugin install muumuu-domain-mcp@pepabo-marketplace
```

### 3. Claude Codeを再起動

インストール後、Claude Codeを再起動すると利用可能になります。

## プラグイン一覧

### pepabo

GMOペパボをより深く知るためのナレッジプラグイン。

| 種別 | 名前 | 説明 |
|------|------|------|
| スキル | `pepabo-guide` | ペパボの企業情報・技術文化・サービス・採用情報などを公式ページから取得して回答 |

ペパボ関連ドメインへのWebFetchはデフォルトで許可されています。

### suzuri-api

SUZURI REST APIを使ったアプリケーション構築ガイド。

| 種別 | 名前 | 説明 |
|------|------|------|
| スキル | `suzuri-api` | APIの認証、リソース構造、実装パターンをまとめたナレッジベース |

## MCP連携

### muumuu-domain-mcp

[ムームードメイン MCPサーバー](https://muumuu-domain.com/muumuu-domain-mcp)連携。インストールすると、AIツールから以下の操作が可能になります。

| 機能 | 内容 |
|------|------|
| ドメイン検索・購入 | 空き検索、価格確認、購入金額・決済方法の確認、購入 |
| ドメイン管理 | 保有ドメインの一覧表示・詳細確認 |
| DNS設定 | レコードの一覧表示・作成・更新・削除 |
| 契約管理 | 契約一覧（更新日・自動更新状態等）の表示・詳細確認 |

初回利用時は `/mcp` からムームードメインへのOAuth認証が必要です。

## ライセンス

本リポジトリのソフトウェアはMITライセンスのもとで公開されています。利用に際して生じたいかなる問題についても、開発元は一切の責任を負いません。詳しくは [LICENSE](LICENSE) をご確認ください。
