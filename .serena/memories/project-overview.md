# AI Agent Marketplace プロジェクト概要

## プロジェクトの目的

Claude Codeプラグインマーケットプレイス（内部用）として、チーム全体でClaude Codeの導入を加速するための事前設定されたプラグインを提供する。

## リポジトリ情報

- **リポジトリ名**: ai-agent-marketplace
- **オーナー**: takemi-ohama
- **ライセンス**: MIT
- **マーケットプレイスURL**: https://github.com/takemi-ohama/ai-agent-marketplace

## 配布コンポーネント

1. **MCPインテグレーションスキル**: GitHub、Serena、BigQuery、Notion MCPの自動セットアップ
2. **カスタムスラッシュコマンド**: 共通タスク用の再利用可能なスラッシュコマンド
3. **サブエージェント**: 異なるドメイン向けの専門AIエージェント
4. **プロジェクトフック**: イベントによってトリガーされる自動ワークフロー

## ディレクトリ構造

```
ai-agent-marketplace/
├── .claude-plugin/
│   └── marketplace.json          # マーケットプレイスメタデータ
├── plugins/
│   ├── mcp-integration/          # MCPセットアップスキル
│   └── slack-notification/       # Slack通知フック
├── README.md                     # プロジェクトドキュメント
├── CLAUDE.md                     # AIエージェント向けガイドライン
└── LICENSE                       # MITライセンス
```

## 現在提供されているプラグイン

### 1. mcp-integration (v1.0.0)
- **説明**: GitHub、Serena、BigQuery、Notion MCPの統合スキル
- **タイプ**: プロジェクトスキル
- **パス**: `./plugins/mcp-integration`

### 2. slack-notification (v1.0.0)
- **説明**: Claude Code作業完了時の自動Slack通知（日本語要約付き）
- **タイプ**: プロジェクトスキル + フック
- **パス**: `./plugins/slack-notification`

## インストール方法

### マーケットプレイスの追加
```bash
/plugin marketplace add
# URL: https://github.com/takemi-ohama/ai-agent-marketplace
```

### プラグインのインストール
```bash
/plugin install mcp-integration@ai-agent-marketplace
/plugin install slack-notification@ai-agent-marketplace
```

## プラグイン開発ガイドライン

### 必須ファイル

1. **plugin.json**: プラグインメタデータ（name, version, description, author）
2. **SKILL.md**: プロジェクトスキルのエントリポイント（YAMLフロントマター必須）

### セマンティックバージョニング

- **MAJOR**: 破壊的変更
- **MINOR**: 後方互換性のある新機能
- **PATCH**: バグフィックス

### セキュリティ要件

- トークンや認証情報をコミットしない
- 環境変数を使用
- `.env`を`.gitignore`に追加
- テンプレートではプレースホルダー値を使用

## 管理タスク

### プラグインの追加
1. プラグインディレクトリ作成
2. plugin.json作成
3. コンテンツ追加（skills/commands/agents）
4. marketplace.jsonに登録
5. ドキュメント作成
6. テスト実行

### プラグインの更新
1. ファイル修正
2. plugin.jsonのバージョンインクリメント
3. コミット＆プッシュ

### プラグインの削除
1. marketplace.jsonから削除
2. プラグインディレクトリ削除（オプション）
3. コミット

## 参考リンク

- [Claude Codeドキュメント](https://docs.claude.com/en/docs/claude-code)
- [プラグインマーケットプレイスドキュメント](https://code.claude.com/docs/ja/plugin-marketplaces)
- [MCP仕様](https://modelcontextprotocol.io)
- [スキルドキュメント](https://docs.claude.com/en/docs/claude-code/skills)
