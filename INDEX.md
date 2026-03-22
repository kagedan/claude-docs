# docs/ — かげだんのClaude運用ナレッジベース

> **目的**: 過去チャットに散在するClaude運用ノウハウを体系化し、
> Claude Code・Cowork・Claude.aiのどこからでも参照できるようにする。
>
> **howto-kb（kagedan/howto-kb）との違い**:
> howto-kbは外部記事の収集・蓄積（980件、自動収集）。
> このdocs/は自分自身の運用ノウハウ・実装手順。両者は共存する。

---

## 実装・運用系（Tips）

| ファイル | 内容 |
|---------|------|
| [cowork-operations.md](cowork-operations.md) | VirtioFS復旧、Projects/Dispatch、1セッション1ファイル原則、PROGRESS.md |
| [claude-code-operations.md](claude-code-operations.md) | CLAUDE.mdの書き方、コンテキスト管理、セッション管理、HANDOVER、GitHub MCP |
| [skill-development.md](skill-development.md) | スキル保存場所マップ、同期問題、skill-creatorの使い方 |
| [mcp-management.md](mcp-management.md) | オンオフ制御、プロジェクトスコープ、コンテキスト消費の管理 |
| [claude-ai-tips.md](claude-ai-tips.md) | chart_display_v0、Reactアーティファクト、Claude CodeとClaude.aiの連携 |

## 設計・方針系（Decisions）

| ファイル | 内容 |
|---------|------|
| [three-layer-workflow.md](three-layer-workflow.md) | Cowork・Claude Code・Claude.aiの使い分け |
| [agent-design.md](agent-design.md) | エージェント役割定義の段階的導入、Output Styles |
| [system-architecture.md](system-architecture.md) | 5階建て設計論、changelog進化ループ、環境横断設計 |

---

## 更新履歴

| 日付 | 内容 |
|------|------|
| 2026-03-22 | 初版作成（8カテゴリ + INDEX.md） |
