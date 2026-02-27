## プラグインコンポーネントの選定基準

プラグイン内のコンポーネント（skill / agent）を選ぶ際の判断フロー。
command は skill に統合済みのため、新規作成時は skill か agent の二択。

### 判断フロー（yes/no で辿る）

```
Q1. 独立したコンテキストウィンドウで実行する必要があるか？
    （メイン会話と分離して、自律的に複数ステップの作業を進める必要があるか）
    │
    ├─ No → skill
    │
    └─ Yes
        │
        Q2. 実行時間が長く、出力量が多いか？
            （短い応答ではなく、コードレビュー全体やデバッグセッションのような長い作業か）
            │
            ├─ No → skill
            │
            └─ Yes
                │
                Q3. 独自のツール制限・権限セットが必要か？
                    （メイン会話とは異なるツールの組み合わせで動く専門的な役割か）
                    │
                    ├─ No → skill
                    │   （allowed-tools frontmatter でツール制御可能）
                    │
                    └─ Yes → agent
```

要するに、Q1〜Q3 がすべて Yes の場合のみ agent。それ以外は skill。

### 判断の補足

skill の守備範囲:
- ナレッジ提供（API仕様、規約、ガイドライン）
- WebFetchで情報を取得して回答する
- 定型操作の手順を提供する
- ユーザーの話題に応じて自動適用される

agent の守備範囲:
- コードレビュー（ファイルを横断して分析し、レポートを出す）
- デバッグ（原因調査→修正→検証を自律的に進める）
- データ分析（大量データを処理して結果を返す）

### よくある迷いどころ

WebFetchでページを取得して回答する → skill
  理由: Q1がNo。メイン会話内で情報取得+回答すれば十分

ツール権限を制限したい → skill
  理由: Q3だけYesでもskill。frontmatterの allowed-tools で制御可能

```yaml
---
name: my-skill
description: ...
allowed-tools: WebFetch(domain:example.com), Read
---
```

### command について

公式ドキュメントでは command と skill は統合済み。新規作成時は skills/ に統一する。
