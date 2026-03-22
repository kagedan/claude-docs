# Skill Development

> スキルの保存場所・開発手法・サーフェス間の同期問題・skill-creatorの使い方

---

## 現状（できていること）

### スキル保存場所の全体マップ

| サーフェス | 保存場所 | 登録方法 |
|---|---|---|
| Claude Code（CLI・VSCode） | ローカル `~/.claude/skills/` | フォルダ直接配置 |
| Claude.ai / Cowork / Excel / PPT | クラウド（Anthropicサーバー） | Customize > Skills からZIPアップロード |
| API | クラウド（ワークスペース単位） | /v1/skills エンドポイント |

### サーフェス間の同期問題
- **自動同期しない**: Claude Codeのスキルとclaude.ai/Coworkのスキルは別管理
- **シンボリックリンクにバグあり**: /コマンドの発見に失敗する
  - 回避策: rsyncでファイルを物理コピー
- **Coworkで作ったスキルをClaude Codeに渡す方法**:
  1. Coworkでスキルファイルを出力
  2. Claude Codeの`~/.claude/skills/`にコピー
  3. またはGitHubリポジトリ経由で共有

### スキルの構造（Anthropic公式 frontend-designスキルの分析から）
- **Design Thinking構造**: 目的 → 方向性 → 制約 → 差別化
- **NEVERリスト**: やってはいけないことを明示
- **AIへの期待値設定**: どこまでやるか・どう判断するかを記述
- descriptionの書き方がスキルの発動精度を左右する

### 暗黙知の言語化
- スキル設計の核は「自分が無意識にやっていることを言葉にする」こと
- 「答えを教える」より「考え方を教える」ほうが汎用性が高い
- Chain-of-Thought（推論の枠組み）をスキルに組み込む
  - 例: DXF管材抽出での「ブロック属性で管種判定 → ライナーグループ化 → 管形状ブロック追加」

### 実運用中のスキル
- osaka-spec-search: 共通仕様書300件超のPDF全文検索
- dxf-pipe-extractor: DXFファイルから管材情報を自動抽出
- piping-diagram-transcriber: 配管図PDFのCSV変換
- jwnet-manifest-dl: 電子マニフェスト自動ダウンロード
- webpage-pdf-downloader: ウェブページからのファイル一括DL

---

## 目標

### changelog（スキル進化ループ）
- スキルフォルダにchangelog.mdを1つ置く
- 手直しが発生するたびに理由を1行記録
- skill-creator Eval機能の指摘事項も追記
- これが5階建て設計論の4〜5階（分析層・進化層）の第一歩

### skill-creatorのEval活用
- Anthropic公式のskill-creatorスキルで品質採点
- 採点結果をchangelogに蓄積 → スキルの改善サイクルを回す

---

## 過去チャットキーワード
`スキル 保存場所 ~/.claude/skills ZIP` / `スキル設計 コツ ベストプラクティス` /
`暗黙知 言語化 Chain-of-Thought` / `shanraisshan best-practice カテゴリ`

## 参照先
- [best-practice/claude-skills.md](https://github.com/shanraisshan/claude-code-best-practice/blob/main/best-practice/claude-skills.md) — スキルのfrontmatter設計
- [orchestration-workflow/](https://github.com/shanraisshan/claude-code-best-practice/tree/main/orchestration-workflow) — Command→Agent→Skillパターン
