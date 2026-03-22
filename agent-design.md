# Agent Design

> エージェント役割定義の段階的導入・Output Stylesの活用

---

## 現状（できていること）

### 役割を与えて専門的観点を得る
- 通常のClaude Codeでも、最初のメッセージで役割を明示すると専門的な回答が得られる
- 例（Cowork）: 「あなたはCoWorkの運用設計の相談相手です。私は建設工事の施工管理業務を効率化したい」
- 役割＋目的を最初に伝えるのがポイント

---

## 目標（★未実装 → 段階的導入）

### 第1段階: CLAUDE.mdに役割明記
**やりたいこと**: プロジェクトのCLAUDE.mdに、そのプロジェクトでのClaudeの役割を書く

**方向性**:
- 例: 竣工書類プロジェクトのCLAUDE.md →
  「あなたは大阪市水道局の公共工事における竣工書類作成の専門家です」
- 役割を書くことで、毎回の指示が簡潔になる
- まずは1つのプロジェクトで試して効果を確認

### 第2段階: `.claude/agents/`にエージェント定義
**やりたいこと**: 複数のエージェント定義を作り、用途に応じて切り替える

**方向性**:
- `.claude/agents/`フォルダにエージェント定義ファイルを配置
- 例: `document-writer.md`（書類作成専門）、`skill-developer.md`（スキル開発専門）
- サブエージェントとしてツール制限・モデル指定も可能
- 要調査: エージェント定義ファイルの具体的なフォーマット

### 第3段階: オーケストレーション
**やりたいこと**: Command → Agent → Skill のパターンで自動的に適切なエージェントが動く

**方向性**:
- コマンドがエントリポイント、エージェントがデータ取得、スキルが成果物作成
- shanraisshan/claude-code-best-practiceのorchestration-workflow/を参照
- 中長期の目標。第1・第2段階が安定してから着手

### Output Stylesの活用
- エージェントの出力スタイルを定義して一貫性を持たせる
- awesome-claude-codeのOutput Styles項目を参照
- 要調査: 具体的な設定方法と業務への適用例

---

## 過去チャットキーワード
`サブエージェント 役割 エージェントチーム` / `Output Styles 出力スタイル` /
`orchestration Command Agent Skill`

## 参照先
- [best-practice/claude-subagents.md](https://github.com/shanraisshan/claude-code-best-practice/blob/main/best-practice/claude-subagents.md) — サブエージェント定義方法
- [orchestration-workflow/](https://github.com/shanraisshan/claude-code-best-practice/tree/main/orchestration-workflow) — オーケストレーションパターン
- [awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) — Output Stylesの項目
