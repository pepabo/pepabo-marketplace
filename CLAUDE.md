## プロジェクト概要

GMOペパボのClaude Codeプラグインを一元管理・配布するマーケットプレイスリポジトリ。
自前実装のプラグインと、外部の優れたプラグインのキュレーション両方を担う。

マーケットプレイス名: `pepabo/pepabo-marketplace`

## ディレクトリ構成

```
pepabo-marketplace/
├── .claude-plugin/
│   └── marketplace.json    # マーケットプレイスメタデータ
├── plugins/
│   ├── pepabo/             # ペパボナレッジプラグイン
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   └── agents/
│   │       ├── pepabo-guide.md
│   │       └── pages-index.md
│   └── suzuri-api/         # SUZURI APIガイドプラグイン
│       ├── .claude-plugin/
│       │   └── plugin.json
│       └── skills/
│           └── suzuri-api/
│               ├── SKILL.md
│               └── references/
│                   └── api-reference.md
├── CLAUDE.md
├── README.md
└── LICENSE
```

## 開発ルール

### ドキュメント参照の優先順位

プラグインやスキルを実装する際は、以下の順に参照する：

1. claude-code-guide エージェント: Claude Code プラグインの最新公式仕様を確認
2. このCLAUDE.md: プロジェクト固有のルール

### プラグイン命名規則

- kebab-case 形式（例: `code-formatter`, `deployment-tools`）
- スペース不可、ユニークな識別子を使用

### バージョニング

- セマンティックバージョニング（MAJOR.MINOR.PATCH）に従う
- プラグインを変更したPRでは、必ず該当プラグインの `version` を上げること
- Claude Codeはプラグインの `version` をキーにキャッシュするため、バージョンを上げないとユーザーに変更が反映されない

### Author（著者）

- plugin.json の author は、そのプラグインにコミットした人の名前を設定する
- `git log --format="%aN" -- plugins/<plugin-name>/ | sort -u` でコミット履歴から著者を取得
- 複数の著者がいる場合はカンマ区切り（例: `"name": "author1, author2"`）

### marketplace.json の更新

プラグインを追加・更新・削除した場合は、`.claude-plugin/marketplace.json` も更新する。

手順:
1. `plugins/<plugin-name>/.claude-plugin/plugin.json` の内容を確認
2. `.claude-plugin/marketplace.json` の `plugins` 配列を更新（アルファベット順）
3. `metadata.version` を patch +1
4. `claude plugin validate .` で検証

### plugin.json の形式

```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "プラグインの説明",
  "author": {
    "name": "作者名"
  },
  "keywords": ["keyword1", "keyword2"],
  "agents": "./agents/",
  "skills": "./skills/",
  "commands": "./commands/"
}
```

コンポーネントパス（agents, skills, commands 等）は省略可能。省略時はデフォルトの位置（`agents/`, `skills/`, `commands/`）から自動検出される。

### plugins 配列のソース形式

ローカルパス（このリポジトリ内）:
```json
{
  "name": "plugin-name",
  "source": "./plugins/plugin-name",
  "description": "プラグインの説明",
  "version": "1.0.0",
  "author": { "name": "作者名" },
  "keywords": ["keyword1"]
}
```

GitHub リポジトリ:
```json
{
  "name": "github-plugin",
  "source": {
    "source": "github",
    "repo": "owner/plugin-repo",
    "ref": "v1.0.0"
  }
}
```

Git URL:
```json
{
  "name": "external-plugin",
  "source": {
    "source": "url",
    "url": "https://github.com/org/plugin-repo.git",
    "ref": "main"
  }
}
```

`ref` でブランチ/タグ、`sha` で特定コミットにピン留め可能。

## よく使うコマンド

```bash
# マーケットプレイスを検証
claude plugin validate .

# プラグインをローカルでテスト
claude --plugin-dir ./plugins/<plugin-name>

# マーケットプレイスをローカルで追加
/plugin marketplace add .

# プラグインをインストール（ユーザー向け）
/plugin marketplace add pepabo/pepabo-marketplace
/plugin install pepabo@pepabo-marketplace
```
