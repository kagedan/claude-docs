# MCP Management

> MCPサーバーの管理・コンテキスト消費の最適化・オンオフ制御

---

## 現状（できていること）

### コンテキスト消費の基本原則
- MCPサーバーを接続すると、**使っていなくても**ツール定義がコンテキストに常に載る
- ツールの数が多いほど「基礎消費量」が増える
- **15個入れて使うのは4個**という教訓 → 本当に必要なMCPだけに絞る

### Claude Desktop と Cowork の関係
- Claude DesktopのMCP設定は**DesktopとCoworkの両方に影響する**
- CoworkはDesktopの中で動く機能なので、MCP設定は共有
- Coworkは200Kコンテキストなので、不要MCP外しが特に重要

### Claude Code と Claude Desktop は完全に別
- Claude CodeのMCP設定（`claude mcp add`）とDesktopのMCP設定は独立
- 同じMCPサーバーを両方で使いたければ、両方に設定が必要
- Claude Code → ターミナルから`claude mcp add`
- Claude Desktop → 設定画面 or `claude_desktop_config.json`を直接編集

### 機能の絞り込み
- Claude Code: `--tools`オプションで使うツールを限定可能
- Claude Desktop: ツール単位の絞り込みは基本できない
  → MCPサーバーの数自体を絞るのが現実的

---

## 目標（★未実装 → 方向性）

### ★ `/mcp`コマンドでセッション内オンオフ
**やりたいこと**: セッション中に特定のMCPを一時的にオン/オフする

**方向性**:
- Claude Codeの`/mcp`コマンドでセッション内の有効/無効を切り替え
- 現在の作業に必要なMCPだけを有効にする運用
- 要調査: `/mcp`コマンドの具体的な構文と挙動

### ★ `.mcp.json`プロジェクトスコープ
**やりたいこと**: プロジェクト単位でMCPを制御する

**方向性**:
- プロジェクトルートに`.mcp.json`を配置して、そのプロジェクトで使うMCPだけを定義
- 例: 竣工書類プロジェクトにはGitHub MCPだけ、スキル開発プロジェクトにはGitHub + テスト系MCP
- 要調査: `.mcp.json`のスキーマと設定方法

### ★ Cowork 200K対策
**やりたいこと**: Coworkのコンテキスト枠を最大限活用する

**方向性**:
- Coworkで普段使わないMCP接続は無効にしておき、必要なときだけオンにする
- GitHub MCP（Coworkのサンドボックスからは接続不可）は常時オフでOK
- 要調査: Coworkの設定画面でのMCPオン/オフの手順

---

## MCPエコシステム参考情報

### 公式連携（claude.ai内蔵）
- Google Calendar、Gmail、Google Drive
- GitHub、Canva、Slack 等

### 実用的なMCPの例
- Adobe系: adb-mcp（Photoshop、Premiere Pro、InDesign等）
- デザイン: Figma（公式）、Blender（非公式）
- プロジェクト管理: Notion、Asana、Jira

### MCPの動向チェック
- GitHubの「awesome-mcp-servers」リポジトリ（2,200+サーバー）
- 新しいサーバーが随時追加されている

---

## 過去チャットキーワード
`MCP コンテキスト 消費 オンオフ` / `MCP 設定 Desktop Cowork 共有` /
`awesome-mcp-servers` / `Adobe Blender Figma MCP`

## 参照先
- [best-practice/claude-mcp.md](https://github.com/shanraisshan/claude-code-best-practice/blob/main/best-practice/claude-mcp.md) — MCP運用ルール
