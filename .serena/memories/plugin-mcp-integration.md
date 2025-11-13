# MCP Integration プラグイン詳細

## プラグイン情報

- **名前**: mcp-integration
- **バージョン**: 1.1.0
- **作者**: takemi-ohama
- **タイプ**: プロジェクトスキル + スラッシュコマンド
- **パス**: `plugins/mcp-integration`

## 概要

Claude Codeプロジェクトに複数のMCP（Model Context Protocol）サーバーを自動セットアップするためのプロジェクトスキル。

**重要**: このスキルは、ユーザーが「MCPサーバーをセットアップして」と言うだけで、自動的に`.mcp.json`と`.env`テンプレートをプロジェクトルートに作成します。ユーザーが手動で作成する必要はありません。

**スマートマージ機能**: 既に`.mcp.json`や`.env`が存在する場合、既存の設定や認証情報を保持したまま、不足している設定だけを追加します。

- `.mcp.json`: 存在しないMCPサーバーのみを追加
- `.env`: 存在しない環境変数のみを追加（既存の値は保持）
- カスタム設定も保持されます

GitHub、Serena、BigQuery、Notion、DBHub、Chrome DevToolsとの統合を提供。

## 含まれるMCPサーバー

### 1. GitHub MCP (HTTP)
- **機能**: リポジトリ管理、PR管理、イシュートラッキング、コード検索
- **認証**: GitHub Personal Access Token
- **公式ドキュメント**: https://github.com/github/github-mcp-server

### 2. Notion MCP (HTTP)
- **機能**: ドキュメント検索、ページ作成、データベース操作
- **認証**: Notion統合トークン
- **公式ドキュメント**: https://mcp.notion.com

### 3. Serena MCP (Local)
- **機能**: セマンティックコード分析、シンボルベース編集、メモリー管理
- **認証**: 不要（ローカル実行）
- **公式ドキュメント**: https://github.com/oraios/serena

### 4. AWS Documentation MCP (Local)
- **機能**: AWSドキュメント検索、コンテンツ読み込み、推奨コンテンツ
- **認証**: 不要（公開ドキュメント）
- **公式ドキュメント**: https://github.com/awslabs/aws-documentation-mcp-server

### 5. BigQuery MCP (Local)
- **機能**: クエリ実行、テーブル操作、スキーマ管理
- **認証**: Google Cloud認証情報
- **公式ドキュメント**: https://github.com/ergut/mcp-server-bigquery

### 6. DBHub MCP (Local)
- **機能**: ユニバーサルデータベースゲートウェイ、スキーマ探索、SQL実行、AI支援SQL生成
- **対応データベース**: PostgreSQL、MySQL、SQL Server、MariaDB、SQLite
- **認証**: データベース接続文字列（DSN）
- **公式ドキュメント**: https://github.com/bytebase/dbhub

### 7. Chrome DevTools MCP (Local)
- **機能**: ブラウザ自動化、入力操作、ナビゲーション、パフォーマンス分析、デバッグ
- **認証**: 不要（ローカル実行）
- **公式ドキュメント**: https://github.com/ChromeDevTools/chrome-devtools-mcp

## ファイル構造

```
plugins/mcp-integration/
├── .claude-plugin/
│   └── plugin.json                    # プラグインメタデータ
├── commands/                          # スラッシュコマンド
│   ├── serena.md                      # 開発の記憶と知見の記録
│   ├── pr.md                          # PR作成
│   ├── fix.md                         # PR修正対応
│   ├── review.md                      # PRレビュー
│   ├── merge.md                       # マージ後クリーンアップ
│   └── clean.md                       # ブランチクリーンアップ
├── README.md                          # ユーザー向けドキュメント
└── skills/
    └── mcp-integration/
        ├── SKILL.md                   # スキルエントリポイント
        ├── mcp-config-template.md     # 完全な.mcp.json設定テンプレート
        ├── mcp-setup-guide.md         # ステップバイステップセットアップ手順
        └── mcp-authentication-guide.md # トークンセットアップ手順
```

## スキルの起動条件

以下のキーワードでスキルが自動起動：
- "MCPサーバーをセットアップ"
- "GitHub/BigQuery/Notionをインテグレート"
- "コード分析を有効化"
- "Serenaを設定"

手動起動:
```
@mcp-integration-skill MCPのセットアップを手伝って
```

## セットアップステップ（自動）

1. **プラグインをインストール**: `/plugin install mcp-integration@ai-agent-marketplace`
2. **スキルを起動**: 「MCPサーバーをセットアップして」と依頼
3. **スキルが自動実行**:
   - `.mcp.json`をプロジェクトルートに自動作成（全MCPサーバーに`envFile`設定を含む）
   - `.env`テンプレートを自動作成（変数名のみ、値は空）
   - `.env`を`.gitignore`に自動追加
   - 前提条件（Python、uvx）を確認
4. **`.env`ファイルに値を入力**: スキルが作成したテンプレートを開いて値を入力
   ```
   GITHUB_PERSONAL_ACCESS_TOKEN=ghp_your_actual_token
   NOTION_API_KEY=secret_your_actual_token  # オプション
   GOOGLE_APPLICATION_CREDENTIALS=/path/to/key.json  # オプション
   ```
5. **Claude Codeを再起動**: MCPサーバーをロード
6. **確認**: 利用可能なツールをチェック

**スキルが自動化する内容:**
- ✅ `.mcp.json`作成または更新（既存ファイルがある場合は不足しているMCPサーバーのみ追加）
- ✅ `.env`テンプレート作成または更新（既存ファイルがある場合は不足している変数のみ追加）
- ✅ `.gitignore`への`.env`追加
- ✅ スマートマージ：既存の設定や認証情報は保持される
- ⏭️ ユーザーは`.env`の空欄に値を入力するだけ

## スラッシュコマンド

プラグインには6つの開発ワークフローコマンドが含まれています：

### 1. /serena - 開発の記憶と知見の記録
- **目的**: AI Agentの操作履歴や失敗を記録し、再発防止
- **実行内容**:
  1. AI Agentの履歴から操作内容を収集
  2. git logやファイル変更から知見を収集
  3. Serena MCPに記憶
  4. 既存記憶の確認・更新
  5. CLAUDE.mdの更新日時を記録

### 2. /pr - PR作成
- **目的**: 変更をコミット・プッシュし、PRを作成
- **実行内容**:
  1. 既存PRの確認（OPENなら追加コミットのみ）
  2. ブランチ確認・切り替え（デフォルトブランチなら新規作成）
  3. 変更をコミット（日本語メッセージ）
  4. プッシュ
  5. PR作成（日本語、Summary + Test plan）
- **注意**: デフォルトブランチでの直接コミット禁止

### 3. /fix - PR修正対応
- **目的**: PRレビューコメントへの対応
- **実行内容**:
  1. レビューコメント確認
  2. 問題点修正
  3. コミット・プッシュ
  4. Copilotにレビュー再依頼
- **方針**: 指摘事項は修正前に仕様を調査し、実施可否を判断

### 4. /review - PRレビュー
- **目的**: 直前PRの専門的レビュー
- **観点**: コード品質、セキュリティ、可読性、保守性、テストカバレッジ
- **結果**: 「Request Changes」または「Approve」

### 5. /merge - マージ後クリーンアップ
- **目的**: PRマージ後のブランチクリーンアップ
- **実行内容**:
  1. PRのマージ確認
  2. 変更をstash
  3. mainブランチ更新
  4. フィーチャーブランチ削除（ローカル・リモート）
  5. stash復元

### 6. /clean - ブランチクリーンアップ
- **目的**: mainマージ済みブランチの削除
- **実行内容**:
  1. `git branch --merged main`で確認
  2. main・現在ブランチを除外
  3. マージ済みブランチ削除（ローカル・リモート）

## 必要な環境変数

### GitHub MCP (必須)
```bash
export GITHUB_PERSONAL_ACCESS_TOKEN="ghp_xxxxxxxxxxxxxxxxxxxx"
```

**必要なスコープ**:
- `repo`: フルリポジトリアクセス
- `read:org`: 組織情報読み取り
- `read:user`: ユーザー情報読み取り
- `read:project`: プロジェクト情報読み取り

### Notion MCP
```bash
export NOTION_API_KEY="secret_xxxxxxxxxxxxxxxxxxxx"
```

### BigQuery MCP (オプション)
```bash
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/service-account-key.json"
```

### DBHub MCP (オプション)
```bash
export DATABASE_DSN="postgres://user:password@localhost:5432/dbname?sslmode=disable"
```

**対応データベース接続文字列:**
- PostgreSQL: `postgres://user:password@localhost:5432/dbname?sslmode=disable`
- MySQL: `mysql://user:password@localhost:3306/dbname`
- SQLite: `sqlite:///path/to/database.db`

### Chrome DevTools MCP
認証不要（ローカルでChromeを制御）

## セキュリティベストプラクティス

1. トークンをコードにコミットしない
2. 環境変数を使用
3. `.env`ファイルを`.gitignore`に追加
4. 最小限のスコープ/権限を使用
5. トークンを定期的にローテーション

## トラブルシューティング

### MCPサーバーが起動しない
1. `.mcp.json`のJSON構文を確認
2. 環境変数が正しく設定されているか確認
3. Claude Codeを再起動
4. トークンの有効性と権限を確認

### 認証エラー
1. トークンの有効期限を確認
2. 必要なスコープ/権限があるか確認
3. トークンフォーマットを確認（`ghp_`で始まる等）

## 提供される機能

### 開発ワークフロー（スラッシュコマンド）
- PR作成・修正・レビュー・マージの自動化
- ブランチ管理とクリーンアップ
- 開発知見の記録と再発防止

### ブラウザ自動化（Chrome DevTools）
- ページナビゲーション
- 要素クリックとフォーム入力
- スクリーンショット取得
- パフォーマンストレーシング
- コンソールログ取得

### データベース操作（DBHub）
- スキーマ探索
- SQL実行
- AI支援SQL生成
- マルチデータベース対応

### コード分析（Serena）
- シンボル検索
- リファレンス検索
- シンボルベース編集
- コードメモリー管理

### GitHub統合
- PR作成・レビュー
- イシュー管理
- コード検索
- リポジトリ操作

### データベース操作（BigQuery）
- SQLクエリ実行
- テーブルスキーマ管理
- データセット操作

### ドキュメント管理（Notion）
- ページ検索
- コンテンツ作成
- データベース操作

## ドキュメントリファレンス

- **README.md**: ユーザー向けメインドキュメント（インストール、環境変数、利用方法）
- **SKILL.md**: スキル概要と起動条件
- **mcp-setup-guide.md**: 詳細セットアップ手順
- **mcp-config-template.md**: 完全な`.mcp.json`テンプレート
- **mcp-authentication-guide.md**: 認証情報取得手順
