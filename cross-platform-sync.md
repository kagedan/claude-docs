# Cross-Platform Sync

> Win/Mac間の設定同期・リポジトリ管理

---

## 現状（できていること）

### 同期しているもの
**GitHubリポジトリ**:
- `kagedan/claude-docs` — 運用ナレッジベース（Win: 作成元、Mac: これからクローン）
- `kagedan/howto-kb` — 外部記事KB（Win: 管理元、Mac: DL済み・未本格運用）

**同期方法**: GitHub経由（push/pull）。GitHub Desktop利用可

### 同期しないもの（意図的に分離）
- **Claudeフォルダ**（`C:\Users\KazuhisaMiyake\Claude\`）: Win専用。業務スキル・仕様書DB・作業フォルダ
- **Mac側の個人プロジェクト**: ブログ・HP・株売買。Win側に持ってこない

### ~/.claude/ の扱い
- WinとMacで別内容。Winベースで作り、Macは個人用途に調整
- 同期していない（する予定もない）。目的が違うので別管理が正解

---

## 設計方針

```
【共有レイヤー】GitHubで同期
  claude-docs/ → 運用ナレッジ（両環境から参照・編集）
  howto-kb/    → 外部記事KB（両環境から参照）

【環境固有レイヤー】同期しない
  Win: ~/.claude/（仕事ルール）+ Claudeフォルダ（業務データ）
  Mac: ~/.claude/（個人ルール）+ 各プロジェクトフォルダ
```

---

## 目標（★未実装）

### ★ claude-docsのMac側クローン
**手順**:
```bash
git clone https://github.com/kagedan/claude-docs.git
```
**運用**: 両環境から編集してpush/pull
**注意**: コンフリクト防止のため、編集する環境を意識する

### ★ howto-kbのMac側本格運用
- 現状DLのみ → 検索・閲覧できる状態にする
- search.pyの動作確認（Python環境の整備が必要な場合あり）

### ★ ~/.claude/ の差分管理
- Win/Macの~/.claude/CLAUDE.mdとrules/の内容を一度比較・記録する
- 差分が意図的なものか更新忘れかを判別
- 共通部分（言語設定、確認スタイル等）は両方に反映
- 環境固有部分は明示的にコメントで「Win専用」「Mac専用」と記載

### ★ 新環境セットアップ時のチェックリスト
PCを買い替えたとき・環境を再構築するときの最低限リスト:
1. Claude Codeインストール
2. `~/.claude/` 設定（CLAUDE.md + rules/）
3. GitHubクローン（claude-docs, howto-kb）
4. MCP接続
5. 動作確認

---

## 過去チャットキーワード
`GitHub 同期 Mac Windows 環境` / `clone push pull` /
`~/.claude/ 設定 グローバル`

## 参照先
- 外部記事なし（howto-kb調査で0件。独自ノウハウとして蓄積する領域）
