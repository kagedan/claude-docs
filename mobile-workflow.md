# Mobile Workflow

> iPhone/iPadからのClaude操作・モバイル→PC引き継ぎフロー

---

## 現状（できていること）

### Claude.aiアプリ
- iPhone/iPadで相談・発想・過去チャット検索に使用

### GitHubアプリ
- Issue作成・確認が可能

---

## 目標（★未実装 → 調査で判明した選択肢）

### ★ Dispatch（2026-03-18リリース） — 最優先で試す
**概要**: スマホからCoworkをリモート操作する機能

**想定ユースケース**:
- 外出先→自宅/職場PCのCoworkに作業を投げる
- 移動中に思いついたタスクをDispatchでPCに投げる運用

**参考記事**:
- 「スマホからClaudeに仕事を投げられる「Dispatch」が登場」(zenn.dev/ap_com)
- 「たった6ステップで使いこなす「Claude Dispatch」完全ガイド」(note.com/no_ai_no_life)
- 「外出先からスマホでAIに仕事を振る。Dispatchを試してわかったこと」(note.com/k158745)

### ★ Channels（Telegram/Discord対応）
**概要**: DiscordからClaude Codeをスマホで操作

**想定ユースケース**:
- 株の自動売買システムの監視に使える可能性あり

**参考記事**:
- 「Claude Code Channelsとは？DiscordからClaude Codeをスマホで操作する設定方法」(note.com/no_ai_no_life)

### ★ Remote Control（2026-03-18リリース）
**概要**: スマホからローカルのClaude Codeセッションをリモート操作

**Dispatchとの違い**: DispatchはCowork向け、Remote ControlはClaude Code向け

**想定ユースケース**:
- Mac側のClaude Codeを外出先から操作する用途に

### ★ その他の選択肢（参考）
- **SSH接続**: iPhoneからSSH経由でClaude Code操作（中級者向け）
  - 参考: 「VPS / iPhoneからSSH接続」(note.com/yellowbirdjp)
- **Telegram連携**: Telegram経由でClaude操作（自作）
  - 参考: 「スマホのTelegramからClaudeに話しかけられる環境を自作した」(zenn.dev/acropapa330)
- **CC Pocket**: モバイル向け閲覧ツール

---

## ぼくだんへの推奨

- まず**Dispatch**から試すのが最も実用的（Coworkの延長で使える）
- Channelsは株の自動売買システムの監視に使える可能性がある
- Remote ControlはMac側のClaude Codeを外出先から操作する用途に

---

## 過去チャットキーワード
`Dispatch スマホ リモート` / `Channels Discord Telegram` /
`モバイル iPhone Claude`

## 参照先
- howto-kb調査結果テーマ3の8件すべて
