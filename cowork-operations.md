# Cowork Operations

> Coworkの運用ノウハウ・トラブルシューティング・新機能情報

---

## 現状（できていること）

### VirtioFS復旧手順
- エラー: `RPC error -1: failed to ensure virtiofs mount: Plan9 mount failed: bad address`
- 原因: CoworkのVM（仮想マシン）とホスト間のファイル共有（VirtioFS）が失敗している
- 復旧: **CoworkFix2.bat**をタスクバーからワンクリックで実行
  - やさしい終了 → 強制終了の2段構え
  - サービス再起動 → Claude Desktop自動再起動
  - 日本語の文字化けでスクリプトが止まる問題を回避済み（全英語化）
  - タスクバーへのピン留めは.batファイル直接ではなく、cmd.exeベースのショートカット経由
- それでも直らない場合: PowerShell管理者でVMデータの完全リセット

### 1セッション1ファイル原則
- 1つのCoworkタスクで複数ファイルを同時に扱うとミスが増える
- 特に数値入力（竣工数量計算書など）では厳守

### PROGRESS.mdで進捗管理
- 長い作業はPROGRESS.mdを作らせて進捗を記録
- セッションが切れても続きから再開できる

### フォルダ選択のコツ
- 作業の起点となる最上位フォルダを選択し、指示文の中でサブフォルダを指定する
- 広すぎる範囲（ホームフォルダ等）は指定しない
- 「+」ボタンからフォルダ外のファイルを参照用に添付可能（読み取り専用）
- Windowsのショートカットやシンボリックリンクはうまく動かない

### CLAUDE.mdの参照
- Claude Codeと違い、Coworkは親ディレクトリのCLAUDE.mdを自動探索しない
- グローバル指示に絶対パスで明示指定するのが確実:
  ```
  作業開始時に以下のCLAUDE.mdを必ず読み、ルールに従うこと：
  1. /Users/kagedan/Claude/CLAUDE.md（共通セキュリティルール）
  2. 作業フォルダ内のCLAUDE.md（存在する場合）
  ```

---

## 新機能（2026年3月時点）

### Projects機能
- タスク・指示・メモリをプロジェクト単位で管理

### Dispatch（3/18リリース）
- スマホからCoworkをリモート操作
- 詳細は [mobile-workflow.md](mobile-workflow.md) を参照

### スケジュールタスク
- `/schedule` による定時実行が可能に（Web版も登場）
- 毎朝のニュース自動収集など、定型タスクの自動化に活用できる

### モデル
- Sonnet 4.6がCoworkのデフォルトモデル

### Microsoft 365統合MCPコネクタ
- Word、Excel、PowerPointなどとの連携が可能に

---

## 注意事項

- Coworkのコンテキストウィンドウは200Kトークン（Claude Codeより小さい）
- 大量ファイルの一括処理はハングのリスクあり（300件で経験済み）
- MCPの接続数を最小限にすることが特に重要（→ mcp-management.md参照）
- CoworkからGitHubへの直接MCP接続はサンドボックス制限で不可
  - 代替: Coworkで作業 → ファイル出力 → GitHub Desktopで同期

---

## 過去チャットキーワード
`virtiofS エラー 復旧 CoworkFix` / `Cowork 運用 トラブル` /
`Cowork フォルダ 選択 階層` / `Cowork 17 実践法 MANIFEST`

## 参照先
- [best-practice/claude-memory.md](https://github.com/shanraisshan/claude-code-best-practice/blob/main/best-practice/claude-memory.md) — CLAUDE.mdの階層読み込み仕組み
