# System Architecture

> 5階建て設計論・changelogによる進化ループ・環境横断設計

---

## 5階建て設計論

### 全体構造

| 階 | 名称 | 内容 | かげだんの状態 |
|----|------|------|--------------|
| 1階 | 設計層（Design Layer） | target.md, voice.md, wins.md（誰に・どう・何が） | ✅ 構築済み（CLAUDE.md、スキル設計） |
| 2階 | 実行層（Action Layer） | skills/, workflows/（自動実行されるスキル群） | ✅ 構築済み（6+スキル運用中） |
| 3階 | 記憶層（Data Layer） | knowledge_base/, feedback/（過去データの蓄積） | ✅ 構築済み（howto-kb, docs/, 共通仕様書DB） |
| 4階 | 分析層（Analytics Layer） | kpi/, analysis/（数値データ・自動分析） | ⬜ 未着手 |
| 5階 | 進化層（Evolution Layer） | changelog/, experiments/（更新履歴・テスト結果） | ⬜ 未着手 |

### 1〜3階でできていること
- **設計層**: CLAUDE.mdでプロジェクトルール定義、スキルのfrontmatter設計
- **実行層**: osaka-spec-search, dxf-pipe-extractor等のスキル群が実務で稼働
- **記憶層**: howto-kb（外部記事980件）、docs/（このナレッジベース）、共通仕様書DB

### 4〜5階に向けた第一歩
- **changelogをスキルフォルダに置く**: 手直し理由を1行記録する運用を始める
- **skill-creator Eval**: 採点結果をchangelogに追記して改善サイクルを回す
- 「テンプレは初日がピーク、システムは毎週進化する」の考え方
- DXF抽出で手直しが必要だった箇所を記録 → スキルのルールに1行追加
  → 次の図面での精度が上がる

---

## changelogによるスキル進化ループ

### 仕組み
```
スキル実行 → 結果確認 → 手直し発生
    ↓
changelog.mdに理由を1行追記
    ↓
スキルのルールに反映
    ↓
次の実行で精度向上
```

### changelog.mdのフォーマット（案）
```markdown
# changelog — [スキル名]

| 日付 | 変更内容 | 理由 |
|------|---------|------|
| 2026-03-22 | 管種判定にNS二受T字管を追加 | φ300でGXとNSの区分ミスが発生 |
| 2026-03-20 | ライナーグループ化ロジック修正 | 単独ライナーが集計漏れ |
```

### v1→v2ループの応用
- 施工計画書や数量計算書: 最初の出力をそのまま使わず「弱点を指摘して改善版を作って」と回す
- 提出書類の品質チェック: 役所に出す前の最終確認としてClaude自身にダメ出しさせる

---

## 環境横断設計

### 現在の環境
- **職場**: Windows PC + Claude Desktop（Cowork含む）+ Claude Code（VSCode）
- **自宅**: Mac mini + Claude Code + Claude.ai
- **モバイル**: iPhone/iPad + Claude.aiアプリ + GitHubアプリ

### 同期の仕組み
- **GitHub**（`kagedan/Claude`リポジトリ）が中心ハブ
- 自宅Mac → push → 職場PC → pull（逆も同じ）
- スキル・docs/・CLAUDE.md すべて同一リポジトリで管理
- iPhoneのGitHubアプリからIssue作成・確認が可能

---

## 過去チャットキーワード
`5階建て 設計層 進化層 changelog` / `テンプレ ピーク システム 進化` /
`Win Mac モバイル 環境 同期` / `GitHub clone push pull`

## 参照先
- [awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) — claude-cost-optimizer（トークンコスト計算）
