# MCP Integration Plugin

Claude CodeプロジェクトにMCP（Model Context Protocol）サーバーを**自動的に**統合するプラグインです。

## 概要

このプラグインをインストールするだけで、複数の強力なMCPサーバーが**自動的に有効化**されます。手動での`.mcp.json`作成や設定は不要です。

**提供されるMCPサーバー：**

- **GitHub MCP**: リポジトリ管理、PR/イシュー操作、コード検索
- **Serena MCP**: セマンティックコード分析、シンボルベース編集
- **Notion MCP**: ドキュメント管理、データベース操作
- **AWS Documentation MCP**: AWS公式ドキュメントへのアクセス
- **BigQuery MCP**: データベースクエリとスキーマ管理
- **DBHub MCP**: ユニバーサルデータベースゲートウェイ（PostgreSQL、MySQL、SQLite等）
- **Chrome DevTools MCP**: Chromeブラウザの自動化・デバッグ・パフォーマンス分析
- **Codex CLI MCP**: コード品質とアーキテクチャ分析、AI支援コードレビュー

## インストール

### 前提条件

- Claude Code がインストール済み
- Python 3.10以上（Serena、BigQuery MCP用）
- `uvx` がインストール済み（`pip install uv`）
- Node.js（DBHub、Chrome DevTools MCP用）
- Codex CLI（Codex CLI MCP用）- オプション

### ステップ1: マーケットプレイスの追加

まず、このプラグインが含まれるマーケットプレイスをClaude Codeに追加します：

```bash
# Claude Codeで実行
/plugin marketplace add https://github.com/takemi-ohama/ai-agent-marketplace
```

### ステップ2: プラグインのインストール

```bash
# Claude Codeで実行
/plugin install mcp-integration@ai-agent-marketplace
```

これだけで、すべてのMCPサーバーが自動的に登録されます！

### ステップ3: .envファイルの作成

プロジェクトルートに `.env` ファイルを作成し、必要な認証情報を設定します。

```bash
# GitHub MCP (必須 - 基本機能用)
# トークン取得: https://github.com/settings/tokens
# 必要なスコープ: repo, read:org, workflow
GITHUB_PERSONAL_ACCESS_TOKEN=

# Notion MCP (オプション - Notion使用時のみ)
# トークン取得: https://www.notion.so/my-integrations
NOTION_API_KEY=

# BigQuery MCP (オプション - BigQuery使用時のみ)
# サービスアカウント作成: https://console.cloud.google.com/iam-admin/serviceaccounts
# 必要なロール: BigQuery Data Editor, BigQuery Job User
GOOGLE_APPLICATION_CREDENTIALS=

# DBHub MCP (オプション - データベース操作用)
# データベース接続文字列 (DSN) - 例:
# PostgreSQL: postgres://USERNAME:PASSWORD@HOST:PORT/DATABASE?sslmode=disable
# MySQL: mysql://USERNAME:PASSWORD@HOST:PORT/DATABASE
# SQLite: sqlite:///PATH/TO/DATABASE.db
DATABASE_DSN=

# 注意: Serena MCP、AWS Documentation MCP、Chrome DevTools MCP、Codex CLI MCPは認証不要
# Codex CLI MCPはインストール必要: https://github.com/openai/codex/releases
# インストール後、'codex login' (ChatGPT) または 'printenv OPENAI_API_KEY | codex login --with-api-key' を実行
```

#### 各トークンの取得方法

**GitHub Personal Access Token（必須）:**
1. https://github.com/settings/tokens にアクセス
2. "Generate new token (classic)" をクリック
3. 以下のスコープを選択:
   - `repo`: リポジトリへのフルアクセス
   - `read:org`: 組織情報の読み取り
   - `workflow`: GitHub Actions ワークフロー更新
4. トークンをコピーして `.env` ファイルに貼り付け

**Notion API Key（オプション）:**
1. https://www.notion.so/my-integrations にアクセス
2. "New integration" をクリック
3. 統合を作成してトークンをコピー
4. Notionでページ/データベースに統合を接続
5. トークンを `.env` ファイルに貼り付け

**BigQuery認証情報（オプション）:**
1. https://console.cloud.google.com/iam-admin/serviceaccounts にアクセス
2. サービスアカウントを作成
3. 以下のロールを付与:
   - `BigQuery Data Editor`
   - `BigQuery Job User`
4. JSONキーをダウンロード
5. キーファイルのパスを `.env` ファイルに記載

**DBHub認証情報（オプション）:**
1. 使用するデータベースの接続情報を取得
2. データベース接続文字列（DSN）を作成
3. DSNを `.env` ファイルに記載

**Codex CLI認証（オプション）:**
Codex CLI MCPを使用する場合、事前に認証が必要です。

**方法1: ChatGPTログイン（推奨）**
```bash
codex login
```
ブラウザが開き、ChatGPTアカウントで認証します。

**方法2: APIキーログイン**
```bash
printenv OPENAI_API_KEY | codex login --with-api-key
```

#### .envファイルの保護

`.env` ファイルには機密情報が含まれるため、必ず `.gitignore` に追加してください：

```bash
echo ".env" >> .gitignore
```

### ステップ4: Claude Codeを再起動

`.env` ファイルに値を入力したら、Claude Codeを再起動してMCPサーバーをロードします。

### ステップ5: 動作確認

以下を確認してください：

1. **MCPツールの確認**
   - Claude Codeで以下のツールプレフィックスが利用可能か確認:
     - `mcp__github__*`
     - `mcp__serena__*`
     - `mcp__notion__*` (Notion設定時)
     - `mcp__awslabs_aws-documentation-mcp-server__*`
     - `mcp__mcp-server-bigquery__*` (BigQuery設定時)
     - `mcp__dbhub__*` (DBHub設定時)
     - `mcp__chrome-devtools-mcp__*`
     - `mcp__codex__*` (Codex CLI設定時)

2. **Serena MCPの初期化（初回のみ）**
   ```
   Serenaプロジェクトをアクティベートして
   ```
   または:
   ```
   mcp__serena__activate_project(".")
   ```

これでセットアップ完了です！

## 特徴

- ✅ **自動統合**: プラグインインストール時にMCPサーバーが自動登録
- ✅ **簡単セットアップ**: `.env`ファイルを作成するだけ
- ✅ **包括的**: 8つの強力なMCPサーバーを一括提供
- ✅ **カスタマイズ可能**: 不要なMCPサーバーは無効化可能
- ✅ **セキュア**: 環境変数で認証情報を管理

## 含まれるMCPサーバー

### 1. GitHub MCP (HTTP)
GitHub APIを通じてリポジトリ操作を実行します。

**主な機能:**
- PR作成・レビュー・マージ
- イシュー管理（作成、更新、検索）
- コード検索
- ブランチ・タグ操作
- リリース管理

**認証:** GitHub Personal Access Token

**公式ドキュメント:** https://github.com/github/github-mcp-server

### 2. Notion MCP (HTTP)
Notionワークスペースとの統合。

**主な機能:**
- ページ検索
- ページ・ブロック作成
- データベース操作
- コンテンツ更新

**認証:** Notion統合トークン

**公式ドキュメント:** https://mcp.notion.com

### 3. Serena MCP (Local)
ローカルコードベースのセマンティック理解と編集。

**主な機能:**
- シンボル検索（クラス、関数、変数等）
- リファレンス検索
- シンボルベース編集（安全なリファクタリング）
- コードメモリー管理
- パターン検索

**認証:** 不要（ローカル実行）

**公式ドキュメント:** https://github.com/oraios/serena

### 4. AWS Documentation MCP (Local)
AWS公式ドキュメントへの高速アクセス。

**主な機能:**
- ドキュメント検索
- コンテンツ読み込み
- 関連ページ推奨

**認証:** 不要（公開ドキュメント）

**公式ドキュメント:** https://github.com/awslabs/aws-documentation-mcp-server

### 5. BigQuery MCP (Local)
Google BigQueryとの統合。

**主な機能:**
- SQLクエリ実行
- テーブル・データセット管理
- スキーマ操作

**認証:** Google Cloud認証情報

**公式ドキュメント:** https://github.com/ergut/mcp-server-bigquery

### 6. DBHub MCP (Local)
ユニバーサルデータベースゲートウェイ。複数のデータベースシステムに対応。

**主な機能:**
- スキーマ探索（テーブル、インデックス、プロシージャ等）
- SQL実行とトランザクションサポート
- AI支援SQL生成
- データベース説明

**対応データベース:**
- PostgreSQL
- MySQL
- SQL Server
- MariaDB
- SQLite

**認証:** データベース接続文字列（DSN）

**公式ドキュメント:** https://github.com/bytebase/dbhub

### 7. Chrome DevTools MCP (Local)
ChromeブラウザをAIエージェントが制御・検査。

**主な機能:**
- 入力自動化（クリック、フォーム入力、ドラッグ等）
- ナビゲーション（ページ管理、遷移）
- エミュレーション（デバイス、ビューポート）
- パフォーマンス分析とトレーシング
- ネットワーク監視
- デバッグ（スクリプト実行、コンソールログ、スクリーンショット）

**認証:** 不要（ローカル実行）

**公式ドキュメント:** https://github.com/ChromeDevTools/chrome-devtools-mcp

### 8. Codex CLI MCP (Local)
コードベースの品質とアーキテクチャをAIで分析。

**主な機能:**
- コード品質・アーキテクチャ分析
- パフォーマンス最適化の提案
- セキュリティ脆弱性検出
- アーキテクチャ設計相談
- コードレビューの自動化

**認証:** 必須（ChatGPTログインまたはAPIキー）

**公式ドキュメント:** https://github.com/openai/codex

## 利用方法

セットアップが完了したら、Claude Codeで自然言語でリクエストするだけです：

```
このリポジトリのオープンなPRを確認して
```

Claude Codeが自動的に適切なMCPツール（GitHub MCP、Serena MCP等）を選択・利用します。

## 開発ワークフローコマンド

GitHub開発フロー（PR作成、レビュー、マージ等）を効率化するスラッシュコマンドは、別プラグイン [**workflow-commands**](../workflow-commands) として提供されています。

以下のコマンドを利用したい場合は、そちらのプラグインも併せてインストールしてください：
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

## MCPサーバーの有効化・無効化

特定のMCPサーバーを無効化したい場合は、Claude Codeの設定から管理できます：

```bash
/mcp
```

または、プロジェクトルートの `.mcp.json` を直接編集して `"disabled": true` を追加：

```json
{
  "mcpServers": {
    "mcp-server-bigquery": {
      "type": "stdio",
      "command": "uvx",
      "args": ["mcp-server-bigquery@latest"],
      "disabled": true
    }
  }
}
```

## トラブルシューティング

### MCPサーバーが起動しない

1. `.env` ファイルがプロジェクトルートに存在するか確認
2. 環境変数が正しく設定されているか確認: `echo $GITHUB_PERSONAL_ACCESS_TOKEN`
3. Claude Codeを完全に再起動
4. ログを確認（Claude Codeの設定から）

### 認証エラー

**GitHub:**
- トークンが有効か確認
- 必要なスコープ（`repo`、`read:org`等）があるか確認
- トークンが`ghp_`で始まるか確認

**Notion:**
- 統合トークンが正しいか確認
- 統合がページ/データベースに接続されているか確認

**BigQuery:**
- サービスアカウントキーのパスが正しいか確認
- サービスアカウントに必要なロールがあるか確認
- プロジェクトでBigQuery APIが有効か確認

**DBHub:**
- データベース接続文字列（DSN）が正しいか確認
- データベースサーバーが起動しているか確認
- ユーザー名とパスワードが正しいか確認

**Codex CLI:**
- Codex CLIがインストールされているか確認（`codex --version`）
- `codex mcp-server`コマンドが正常に実行できるか確認
- 認証済みか確認（`codex login`）

### Serena MCPが動作しない

```bash
# Python バージョンを確認
python --version  # 3.10以上である必要あり

# uvx を確認
uvx --version

# uvx を再インストール
pip install --upgrade uv
```

### ツールが表示されない

1. Claude Codeを再起動
2. `.env` ファイルがプロジェクトルートにあるか確認
3. 環境変数が正しく設定されているか確認
4. MCP設定が正しくロードされているか、Claude Codeのログで確認

## セキュリティのベストプラクティス

- ✅ 環境変数でトークンを管理
- ✅ `.env` ファイルを `.gitignore` に追加
- ✅ 最小限のスコープ/権限を使用
- ✅ トークンを定期的にローテーション
- ❌ トークンをコードやドキュメントにコミットしない
- ❌ 本物に見えるトークン例を使用しない

## サポート

問題が発生した場合：
1. 上記のトラブルシューティングセクションを確認
2. 各MCPサーバーの公式ドキュメントを参照
3. GitHubリポジトリでイシューを作成

## ライセンス

MIT License

## 作者

takemi-ohama - https://github.com/takemi-ohama

## 貢献

プルリクエストを歓迎します。大きな変更の場合は、まずイシューを開いて変更内容を議論してください。
