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

### 3. Claude Codeを再起動

インストール後、Claude Codeを再起動すると利用可能になります。

## プラグイン一覧

### pepabo

GMOペパボをより深く知るためのナレッジプラグイン。

| 種別 | 名前 | 説明 |
|------|------|------|
| エージェント | `pepabo-guide` | ペパボの企業情報・技術文化・サービス・採用情報などを公式ページから取得して回答 |

ペパボ関連ドメインへのWebFetchはデフォルトで許可されています。

### suzuri-api

SUZURI REST APIを使ったアプリケーション構築ガイド。

| 種別 | 名前 | 説明 |
|------|------|------|
| スキル | `suzuri-api` | APIの認証、リソース構造、実装パターンをまとめたナレッジベース |

## ライセンス

本リポジトリのソフトウェアはMITライセンスのもとで公開されています。利用に際して生じたいかなる問題についても、開発元は一切の責任を負いません。詳しくは [LICENSE](LICENSE) をご確認ください。
