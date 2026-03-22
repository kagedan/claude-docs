# Claude Code Operations

> Claude Codeの運用ノウハウ・CLAUDE.md設計・セッション管理

---

## 現状（できていること）

### CLAUDE.mdの書き方
- **150行以内に抑える**（60行が理想とも言われる）
- 長すぎると逆効果（コンテキストを圧迫し、指示の優先度が曖昧になる）
- 階層読み込みの仕組み:
  - `~/.claude/CLAUDE.md` → 全プロジェクト共通（セキュリティルール等）
  - プロジェクトルートの`CLAUDE.md` → プロジェクト固有のルール
  - サブフォルダの`CLAUDE.md` → フォルダ固有の追加ルール
  - 親→子の順に自動で読み込まれる（Claude Codeのみ。Coworkは自動探索しない）
- セキュリティルールは最上位のCLAUDE.mdに書く（どのサブフォルダでも常に有効）

### HANDOVERファイル
- コンテキスト50%到達時に自動生成するルールをCLAUDE.mdに記載
- セッションを超えた作業の継続性を確保
- 内容: 背景・目的、確定した方針、次にやること、検索キーワード

### GitHub MCP連携
- Claude Code（VSCode拡張経由）からGitHubリポジトリに直接コミット・プッシュ可能
- リポジトリ: `kagedan/Claude`
- セットアップ: GitHub PAT作成 → Node.js・Claude Code CLIインストール → MCP接続

### planモードから始める
- いきなり作業させず、まず計画を確認してから実行
- タスク完了したらすぐcommit

### ログ保存場所（Windows）
- 全体履歴: `%USERPROFILE%\.claude\history.jsonl`
- セッション詳細: `%USERPROFILE%\.claude\projects\` 配下にJSONL

---

## 目標（★未実装 → 実装手順つき）

### ★ `/compact` 50%ルール
**やりたいこと**: コンテキストが50%に達したら`/compact`を実行する運用ルール

**実装手順**:
1. CLAUDE.mdに以下を追記:
   ```
   ## コンテキスト管理ルール
   - コンテキストが50%に達したら `/compact` を実行すること
   - 限界まで引っ張らない（品質が劣化する前に圧縮）
   - `/compact`実行前にHANDOVERファイルを生成すること
   ```
2. 目視の目安: Claude Codeのステータスバーでコンテキスト使用量を確認

### ★ `/clear`ルール
**やりたいこと**: セッションをクリアするタイミングの基準を決める

**実装手順**:
1. CLAUDE.mdに以下を追記:
   ```
   ## セッションクリアの基準
   - タスクが完全に完了し、次のタスクが別テーマの場合 → /clear
   - /compact後もコンテキストが圧迫されている場合 → HANDOVER作成後に/clear
   - 同一テーマの作業が続く場合 → /clearせず継続
   ```

### ★ `/rename`運用
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

## 過去チャットキーワード
`CLAUDE.md 書き方 階層 自宅Mac` / `HANDOVER コンテキスト 50%` /
`/rename /resume セッション` / `GitHub MCP 接続 同期`

## 参照先
- [best-practice/claude-memory.md](https://github.com/shanraisshan/claude-code-best-practice/blob/main/best-practice/claude-memory.md) — CLAUDE.mdの階層読み込み
- [best-practice/claude-commands.md](https://github.com/shanraisshan/claude-code-best-practice/blob/main/best-practice/claude-commands.md) — /コマンド一覧
