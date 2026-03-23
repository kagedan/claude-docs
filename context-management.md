# Context Management

> コンテキストウィンドウ最適化・トークン削減・セッション管理

---

## 現状（できていること）

### コンテキストウィンドウの基礎
- **Claude Code（Opus 4.6）**: 1Mトークン（2026年3月〜）
- **Cowork**: 200Kトークン（Claude Codeより小さい。MCP多い環境では特に重要）
- **大きいコンテキスト ≠ 良い出力**。ノイズが増えると品質が下がる
  - MRCRベンチマーク: 256K(25%)時の精度93% → 1M(100%)で76%に低下
  - Lost in the middle問題: コンテキスト中盤の情報は無視されやすい
  - 重要な指示はCLAUDE.md（先頭）か直近の会話（末尾）に置く

### コンテキストを消費する3大要因
1. **CLAUDE.md + rules/ + MEMORY.md**（毎セッション自動読み込み = 固定コスト）
   - CLAUDE.mdは20〜30行が現実的な目安（32行論文、推論コスト20%増の報告）
   - auto-memory（MEMORY.md）は200行制限あり
   - → 詳細は [claude-code-operations.md](claude-code-operations.md)
2. **MCP接続**（「コンテキスト税」）
   - 接続しているだけで、使わなくてもツール定義がコンテキストに常駐する
   - 15個入れて使うのは4個、は典型的な無駄 → 必要最小限に
   - → 詳細は [mcp-management.md](mcp-management.md)
3. **ファイル読み込み + 会話履歴**（セッションが長いほど蓄積 = 変動コスト）
   - 大量ファイルの一括読み込みは避ける（_MANIFEST.mdのTier構造で制御）
   - セッションが長くなるほど会話履歴が膨らむ → /compact で圧縮

### HANDOVERファイル
- コンテキスト使用量が閾値に達した時にHANDOVERファイルを生成するルールをCLAUDE.mdに記載
- セッションを超えた作業の継続性を確保
- 内容: 背景・目的、確定した方針、次にやること、検索キーワード

### セッション管理の基本コマンド
- `/compact` — コンテキストを圧縮（要約してトークンを削減）
- `/clear` — セッションをクリア（コンテキストを完全にリセット）
- `/rename` — セッションに名前を付ける（後から探しやすくする）

### トークン削減テクニック
- **SKILL.mdの最適化**: 冗長な説明を除去し要点のみ記述 → 40%削減の実績
- **MCP接続の整理**: 不要なMCPを外す → 最大90%削減（ツール定義が消えるため）
- **短いCLAUDE.md**: 20〜30行を目安に。超えたらrules/やskills/に分離
- **Coworkでは特にMCP接続数を絞る**（200Kコンテキスト制約）

### コンテキストエンジニアリングの実践
- Anthropic公式ガイドが提唱: 「プロンプトエンジニアリング」→「コンテキストエンジニアリング」へ
- プロンプトの書き方だけでなく、何をコンテキストに入れて何を外すかが品質を決める
- 長時間エージェントのパターン:
  - セッション前半の指示は後半で薄れる（Lost in the middle）
  - 重要な指示はCLAUDE.mdの先頭と末尾に配置する

---

## 目標（実装済み）

### /compact 早期実行ルール（実装済み — workflow.mdに反映）
**やりたいこと**: コンテキストが劣化する前に`/compact`を実行する運用ルール

**背景**: 大きいコンテキストは精度を下げる。劣化してからcompactしても「混乱した状態」を圧縮するだけで逆効果。早めに実行する方が品質を保てる。
- MRCRベンチマーク: 256K(25%)で精度93% → 1M(100%)で76%に低下
- GitHub Issue #34685: 20%付近から劣化開始の報告

**実装手順**:
1. CLAUDE.mdに以下を追記:
   ```
   ## コンテキスト管理ルール
   - 200Kコンテキスト（標準）: 60%（120K）で /compact を実行すること
   - 1Mコンテキスト（Opus 4.6）: 30〜40%（300K〜400K）で /compact を実行すること
   - 限界まで引っ張らない（劣化後のcompactは混乱を圧縮するだけで逆効果）
   - /compact実行前にHANDOVERファイルを生成すること
   ```
2. 目視の目安: Claude Codeのステータスバーでコンテキスト使用量を確認

### /clearルール（実装済み — workflow.mdに反映）
**やりたいこと**: セッションをクリアするタイミングの基準を決める

**実装手順**:
1. CLAUDE.mdに以下を追記:
   ```
   ## セッションクリアの基準
   - タスクが完全に完了し、次のタスクが別テーマの場合 → /clear
   - /compact後もコンテキストが圧迫されている場合 → HANDOVER作成後に/clear
   - 同一テーマの作業が続く場合 → /clearせず継続
   ```

### /rename運用（実装済み — workflow.mdに反映）
**やりたいこと**: セッションに名前を付けて後から探しやすくする

**実装手順**:
1. タスク開始時に`/rename`で作業内容を反映した名前を付ける
   - 例: `dxf-pipe-extractor-v2`, `docs-skill-development`
2. CLAUDE.mdに命名規則を追記:
   ```
   ## セッション命名規則
   - /rename でセッション名を「スキル名-作業内容」の形式で付けること
   - 例: osaka-spec-search-bugfix, docs-index-update
   ```

---

## 実運用ガイド（2026-03-23 実装済み）

以下のルールは `~/.claude/rules/workflow.md` および Coworkグローバル指示に反映済み。

### Claude Code での運用

**コンテキスト使用量の確認方法:**
- `/context` と入力すると使用量（%）と内訳が表示される
- VSCode拡張版ではステータスライン常時表示は未対応（CLI版のみ）
- 作業の区切りごとに自分で `/context` を打って確認する習慣をつける

**compact のタイミング:**
- 50%に達したら手動で `/compact` を実行する（auto-compactの80%を待たない）
- compact前にHANDOVERファイルを `_progress/` に生成する
  内容: 背景・目的、確定した方針、次にやること、変更済みファイル一覧
- compact時の保持指示はプロジェクトCLAUDE.mdに記載済み

**/clear の判断基準:**
- タスクが完了し、次のタスクが別テーマ → /clear
- compact後もコンテキストが圧迫される → HANDOVER作成後に /clear
- 同一テーマの作業が続く → /clear せず継続

**/rename の運用:**
- タスク開始時に `/rename` でセッション名を付ける
- 命名規則: スキル名-作業内容（例: osaka-spec-search-bugfix, docs-index-update）

### Cowork での運用

**コンテキスト使用量の確認方法:**
- **確認する手段がない。** /context のようなコマンドもなく、表示もない
- 200Kの上限に近づくと自動compactが走るが、ユーザーには見えない

**現実的な運用:**
- タスクが完了したら新しいセッションで次のタスクを始める（最も確実）
- 長いタスクの途中では `_progress/` に途中経過を保存しておく
- 1セッションで複数の無関係なタスクを詰め込まない

### 根拠となる情報源

| ソース | 推奨値 | 備考 |
|--------|-------|------|
| shanraisshan/claude-code-best-practice | 最大50%で手動compact | 2026年3月時点で活発に更新中。200K/1M区別なし |
| GitHub Issue #34685 | 20%で劣化開始、48%で再起動推奨 | Opus 4.6の1Mセッションでの報告 |
| MRCRベンチマーク（Issue #35296） | 256K(25%)で精度93%、1M(100%)で76% | 劣化カーブの変曲点が40〜50% |
| SitePoint記事 | 60%（120K以内に抑える） | 200Kコンテキスト想定 |
| Anthropic公式 Best Practices | タスク間で/clear推奨 | compact時の保持指示をCLAUDE.mdでカスタマイズ可能 |

**結論:** 200Kなら50〜60%、1M（Opus）なら30〜40%が安全圏。
劣化してからのcompactは「混乱を圧縮するだけ」で逆効果。早めに手動実行する。

---

## 過去チャットキーワード
`HANDOVER コンテキスト 50%` / `/compact /clear /rename セッション管理` /
`トークン 削減 最適化` / `MCP コンテキスト 消費` /
`Lost in the middle` / `コンテキストエンジニアリング`

## 参照先
- [claude-code-operations.md](claude-code-operations.md) — CLAUDE.mdの書き方、auto-memory
- [mcp-management.md](mcp-management.md) — MCP接続によるコンテキスト消費
- [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) — Anthropic公式
- [Prompt engineering for Claude's long context window](https://www.anthropic.com/news/prompting-long-context) — Anthropic公式
- [best-practice/claude-commands.md](https://github.com/shanraisshan/claude-code-best-practice/blob/main/best-practice/claude-commands.md) — /コマンド一覧
