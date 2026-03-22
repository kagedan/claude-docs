# Three-Layer Workflow

> Cowork・Claude Code・Claude.aiの三層構成と使い分けの方針

---

## 三層構成の定義

| レイヤー | ツール | 役割 | 得意なこと |
|---------|--------|------|-----------|
| 実務運用 | **Cowork** | 完成スキルの実行、日常の書類作業 | ファイル操作の「見える化」、スキルの動作確認 |
| 開発・管理 | **Claude Code** | スキル開発、GitHub管理、MCP接続、docs/管理 | コマンド一発の効率的な操作、大きなコンテキスト |
| 相談・発想 | **Claude.ai** | 方針相談、新機能体験、学習、ナレッジ整理 | スマホ・iPadで気軽にアクセス、過去チャット検索 |

---

## 使い分けの具体例

### Cowork → こういう作業に使う
- 共通仕様書検索（osaka-spec-search）
- DXFからの管材抽出（dxf-pipe-extractor）
- 竣工書類の数量計算（Excel操作）
- 電子マニフェストのダウンロード
- スタッフに見せながらの作業

### Claude Code → こういう作業に使う
- スキルの新規開発・修正
- GitHubへのコミット・プッシュ
- CLAUDE.mdの編集
- MCPサーバーのセットアップ
- docs/ナレッジベースの管理
- 環境のセットアップ・トラブルシュート

### Claude.ai → こういう場面で使う
- 「こういうことがしたいんだけど、どうすればいい？」の相談
- 新しいツール・機能の情報収集と体験
- 過去チャットを検索してナレッジの掘り起こし
- 引継書（HANDOVER）やまとめMarkdownの作成
- iPadやiPhoneからの作業（移動中・自宅）

---

## レイヤー間の引き継ぎ

### Claude.ai → Claude Code
- claude.aiでMarkdownにまとめてファイル化 → Claude Codeに渡す
- またはテキストをコピー → Claude Codeに貼り付け
- チャットURLは共有不可（中身を読めない）

### Cowork → Claude Code
- Coworkでファイルを出力 → Claude Codeの作業フォルダにコピー
- スキルの受け渡し: Coworkで作ったスキルを`~/.claude/skills/`に配置
- GitHubへの同期: GitHub Desktopで手動push

### Claude Code → Cowork
- GitHubにpush → Coworkの作業フォルダにpull（GitHub Desktop経由）
- スキルのZIP化 → claude.aiのCustomize > SkillsからZIPアップロード

---

## Win/Mac環境横断

- **GitHub経由で同期**: 自宅Mac → push → 職場PC → pull（逆も同じ）
- Claude Codeが両環境にあるので、どちらでも同じ操作が可能
- スキルもdocs/も同じリポジトリ（`kagedan/Claude`）で管理

---

## 過去チャットキーワード
`三層構成 使い分け Cowork Claude Code` / `Claude Code Cowork 使い分け デビュー` /
`GitHub 同期 Mac Windows 環境`

## 参照先
- 特になし（自分独自の運用体系のため）
