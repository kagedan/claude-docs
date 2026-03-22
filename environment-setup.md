# Environment Setup

> Win/Mac各環境のClaude初期セットアップ・設定管理

---

## 現状（できていること）

### Win（職場）環境 — 公共工事の書類作成用

**Claude Code（VSCode拡張）**
- 3分レイアウト（Explorer / Editor / Claude Codeパネル）で運用中
- settings.json: 許可コマンド設定済み

**Cowork（Claude Desktop）**
- VirtioFS復旧バッチ整備済み（→ cowork-operations.md）
- グローバル指示設定済み

**~/.claude/ 構成**
- `CLAUDE.md`: 英語・見出しレベル（約20行）
- `rules/`: safety.md, workflow.md, task-management.md, quality.md（日本語・詳細）
- 階層読み込み確認済み（グローバル→rules→プロジェクト→作業フォルダ）

**プロジェクトCLAUDE.md**
- `C:\Users\KazuhisaMiyake\Claude\CLAUDE.md` で各層構成の動作確認済み

**MCP・スキル**
- GitHub MCP接続済み
- スキル6つ以上を実運用中（osaka-spec-search, dxf-pipe-extractorなど）

### Mac（自宅）環境 — ブログ・HP・個人プロジェクト用

**Claude Code**
- インストール済み、VSCode使用

**~/.claude/**
- Winをベースに調整（仕事関係を削除、個人用途を追加）

**用途**
- ブログ執筆（Markdown→公開）
- HP作成（デザイン業・ポートフォリオ）
- 株売買自動化

**GitHubリポジトリ**
- howto-kb: DL済み・未設定
- claude-docs: 未DL

### 展開の設計原則
- Winと Macは「同じ環境の複製」ではなく、**目的が違う別環境**
- 共有するもの: claude-docs（GitHub）、howto-kb（GitHub）
- 共有しないもの: Win側のClaude業務フォルダ・スキル・仕様書DB / Mac側のブログ・HP・株プロジェクト

---

## 目標（★未実装）

### ★ Win: 最新機能の反映
- auto-memory（MEMORY.md）の確認と設定
- Hooks設定の見直し（21イベント対応）
- CLAUDE.mdの行数見直し（現状約20行→最新の知見と秤量）
- settings.jsonのallowReadサンドボックス設定確認

### ★ Mac: セットアップの仕上げ
- claude-docsリポジトリのクローン
- howto-kbの正式セットアップ（現在はDLのみ）
- ブログ執筆用のプロジェクトCLAUDE.md作成
- HP作成用のプロジェクトCLAUDE.md作成

### ★ 両環境共通
- Claude Codeのバージョン確認と更新手順のドキュメント化
- 各環境のsettings.jsonの差分を把握・記録

### ★ 新環境セットアップ時のチェックリスト
PCを買い替えたとき・環境を再構築するときの最低限リスト:
1. Claude Codeインストール
2. ~/.claude/ 設定（CLAUDE.md + rules/）
3. GitHubクローン（claude-docs, howto-kb）
4. MCP接続
5. 動作確認

---

## 参考にすべき調査記事

- 「CLAUDE.mdに本当は何を書くべきなのか」(zenn.dev/cureapp)
- 「Claude Codeの全体像：5つの実行環境」(zenn.dev/picnic)
- 「Windows版セットアップ解説」(note.com/yamachas0)
- 「Claude Codeを手懐ける — カスタマイズ戦略」(zenn.dev/76hata)

## 過去チャットキーワード
`CLAUDE.md 書き方 階層 自宅Mac` / `settings.json 許可コマンド` /
`VSCode Claude Code セットアップ 3分`

## 参照先
- [best-practice/claude-memory.md](https://github.com/shanraisshan/claude-code-best-practice/blob/main/best-practice/claude-memory.md)
- [Claude Code公式: Memory](https://code.claude.com/docs/ja/memory)
