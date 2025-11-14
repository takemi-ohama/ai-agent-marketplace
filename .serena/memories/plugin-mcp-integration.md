# MCP Integration プラグイン詳細

## プラグイン情報

- **名前**: mcp-integration
- **バージョン**: 1.1.0
- **作者**: takemi-ohama
- **タイプ**: プロジェクトスキル + スラッシュコマンド
- **パス**: `plugins/mcp-integration`

## 概要

Claude Codeプロジェクトに複数のMCP（Model Context Protocol）サーバーを自動統合するプラグイン。

**重要**: このプラグインは、インストール時に自動的にMCPサーバーを登録します。`.mcp.json`ファイルはプラグイン内に定義されており、ユーザーは`.env`ファイルを作成するだけで利用可能です。

**バージョン2.0の変更点**: 
- プラグイン内に`.mcp.json`を直接定義（スキル経由不要）
- プラグインインストール時にMCPサーバーが自動で有効化
- skillsディレクトリは削除済み

GitHub、Serena、BigQuery、Notion、DBHub、Chrome DevTools、Codex CLIとの統合を提供。

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
├── .mcp.json                          # MCPサーバー定義（自動統合）
└── README.md                          # ユーザー向けドキュメント
```

**v2.0の簡略化**: skillsディレクトリとcommandsディレクトリは削除されました。MCPサーバーはプラグイン内の`.mcp.json`で直接定義されます。

## セットアップステップ（v2.0）

1. **プラグインをインストール**: `/plugin install mcp-integration@ai-agent-marketplace`
2. **MCPサーバーが自動で有効化**: プラグイン内の`.mcp.json`により、すべてのMCPサーバーが自動登録される
3. **`.env`ファイルを作成**: プロジェクトルートに`.env`ファイルを作成し、必要な認証情報を入力
   ```
   GITHUB_PERSONAL_ACCESS_TOKEN=ghp_your_actual_token
   NOTION_API_KEY=secret_your_actual_token  # オプション
   GOOGLE_APPLICATION_CREDENTIALS=/path/to/key.json  # オプション
   DATABASE_DSN=postgres://user:password@localhost:5432/db  # オプション
   ```
4. **`.env`を`.gitignore`に追加**: セキュリティのため
   ```bash
   echo ".env" >> .gitignore
   ```
5. **Claude Codeを再起動**: MCPサーバーをロード
6. **確認**: 利用可能なツールをチェック

**v2.0での変更点:**
- ✅ プラグインインストール時にMCPサーバーが自動登録
- ✅ スキル経由不要（`.mcp.json`はプラグイン内に定義済み）
- ✅ ユーザーは`.env`ファイルを作成するだけ
- ✅ シンプルで直感的なセットアップ

## 開発ワークフローコマンド

**v2.0の変更**: スラッシュコマンドは別プラグイン [workflow-commands](../workflow-commands) に分離されました。

以下のコマンドを利用したい場合は、workflow-commandsプラグインをインストールしてください：
- `/serena` - 開発の記憶と知見の記録
- `/pr` - PR作成
- `/fix` - PR修正対応
- `/review` - PRレビュー
- `/merge` - マージ後クリーンアップ
- `/clean` - ブランチクリーンアップ

**インストール方法：**
```bash
/plugin install workflow-commands@ai-agent-marketplace
```

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
- **.mcp.json**: MCPサーバー定義（プラグイン内に含まれる）

**v2.0の簡略化**: skillsディレクトリは削除されました。すべての情報はREADME.mdに統合されています。
