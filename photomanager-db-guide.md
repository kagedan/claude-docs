# PhotoManager DB操作ガイド

## 概要
PhotoManager 20（株式会社ワイズ製）のデータベース（Photo.pmg）をPythonから直接操作するための技術ガイド。
工事写真帳の仕分け・タイトル入力等の自動化に使用する。

## 検証日
2026年3月24日（R02-016 我孫子枝線500mm配水管改良工事のデータで検証済み）

## DB基本情報

| 項目 | 内容 |
|------|------|
| ファイル名 | Photo.pmg |
| 形式 | Microsoft Access MDB (Jet) |
| ヘッダ | `\x00\x01\x00\x00Standard Jet DB` |
| 接続方法 | pyodbc + "Microsoft Access Driver (*.mdb, *.accdb)" |
| テーブル数 | 53 |

## フォルダ構造

工事写真帳のデータフォルダの標準構成:
- Photo.pmg — メインDB
- ~pm1.bak — DBバックアップ
- dekigata.dt — 出来形データ（独自バイナリ）
- PHOTO/PIC/ — 写真本体（JPG、P0000001.JPG〜の連番）
- PHOTO/PICM/ — 中サムネイル（.TNM）
- PHOTO/PICS/ — 小サムネイル（.TNS）
- PHOTO/DRA/ — 参考図用
- PHOTO/FIG/ — 豆図用
- Settings/Album.pms — アルバムフォント設定
- Settings/Print.pms — 印刷レイアウト設定
- CLOUD/Kokuban/ — 電子黒板画像

## 主要テーブル

### tPicInfo（写真情報、メインテーブル）
71カラム。1レコード＝1枚の写真。

主要カラム:
- picinfoid: 主キー（連番）
- picfilename: 内部ファイル名（P0000001.JPG）→ PHOTO/PIC/内の実ファイル
- picfileorg: 元ファイル名（カメラの元ファイル名）
- picfiletitle / picfiletitleRTF: 写真タイトル
- place / placeRTF: 撮影箇所
- folder1〜folder5 / folder1RTF〜folder5RTF: 分類（大分類・区分・写真区分・工種・細別）
- folderid, FolderID1, FolderID2: フォルダID（全て同値）
- shootdate: 撮影日（YYYYMMDD形式）
- managedval / managedvalRTF: 施工管理値
- viewno: フォルダ内表示順

### tPicInfoFolder（フォルダツリー）
- folderid: 主キー
- parentid: 親フォルダID
- level: 階層レベル（1〜10）
- namae: フォルダ名
- viewno: 表示順

### tAlbumInfo / tAlbumPage / tAlbumPics（アルバム関連）
- アルバム定義、ページ構成、写真割当て

### tPicInfoHash（ハッシュ）
- SHA-1ハッシュ（改ざん検知用）
- 写真ファイル自体を変更しない限り更新不要

### tPicInfoTableRow（管理値）
- komoku: 項目名
- sekkei: 設計値
- jissoku: 実測値

## 書き込みルール（検証済み）

### ルール1: プレーンテキストとRTFは常にセットで更新
- 一覧表示: プレーンテキストフィールドを参照
- 詳細パネル: RTFフィールドを参照
- 片方だけ更新すると不一致になるため、必ず両方を更新する

### ルール2: RTFフォーマット
```
{\rtf1\ansi\ansicpg932\deff0{\fonttbl{\f0\fnil\fcharset128 \'82\'6c\'82\'72 \'82\'6f\'96\'be\'92\'a9;}}
{\colortbl ;\red0\green0\blue0;}
\uc1\pard\cf1\lang1041\f0\fs20【テキスト本文】}
```
- フォント: MS P明朝 (charset128 = Shift-JIS)
- サイズ: 10pt (fs20)
- 日本語文字: cp932の各バイトを \'XX 形式に変換
- ASCII文字: そのまま
- 空フィールド: `{\rtf1\ansi\ansicpg932\deff0\deflang1033\deflangfe1041\uc1 }`

### ルール3: フォルダ移動に必要なフィールド
- folderid, FolderID1, FolderID2: 全て同じフォルダIDを設定
- folder1〜folder5 + 各RTF: フォルダ階層の上位5階層テキスト
- viewno: フォルダ内の表示順（連番、重複なし）
- 既存フォルダへの移動ならtPicInfoFolderの変更は不要

### ルール4: PhotoManagerは閉じてから書き込む
同時アクセスするとDBがロックされる。

## RTF変換関数（検証済み）

```python
def text_to_rtf(plain_text):
    """プレーンテキストをPhotoManager互換のRTF文字列に変換"""
    rtf_body = ""
    for char in plain_text:
        encoded = char.encode("cp932")
        if len(encoded) == 1 and 0x20 <= encoded[0] <= 0x7E:
            rtf_body += char
        else:
            for b in encoded:
                rtf_body += "\\'{:02x}".format(b)

    font_hex = "\\'82\\'6c\\'82\\'72 \\'82\\'6f\\'96\\'be\\'92\\'a9"
    header = r"{\rtf1\ansi\ansicpg932\deff0{\fonttbl{\f0\fnil\fcharset128 " + font_hex + r";}}"
    color = r"{\colortbl ;\red0\green0\blue0;}"
    body = r"\uc1\pard\cf1\lang1041\f0\fs20" + rtf_body + "}"
    rtf = header + "\r\n" + color + "\r\n" + body + "\r\n"
    return rtf
```

## Python接続サンプル

```python
import pyodbc

def connect_photomanager(pmg_path):
    """PhotoManager DBに接続"""
    conn_str = (
        r"Driver={Microsoft Access Driver (*.mdb, *.accdb)};"
        f"DBQ={pmg_path};"
    )
    return pyodbc.connect(conn_str)

# 使用例
conn = connect_photomanager(r"C:\path\to\工事写真帳\Photo.pmg")
cursor = conn.cursor()

# 読み取り
cursor.execute("SELECT picfiletitle, folder3, folder4, folder5 FROM tPicInfo WHERE picinfoid = 1")
row = cursor.fetchone()

# 書き込み（プレーン + RTF）
new_title = "テスト写真"
cursor.execute(
    "UPDATE tPicInfo SET picfiletitle = ?, picfiletitleRTF = ? WHERE picinfoid = ?",
    new_title, text_to_rtf(new_title), 1
)
conn.commit()
conn.close()
```

## 自動化ロードマップ

### Phase 1: DB読み書き基盤 ✅完了
- pyodbcでの読み取り・書き込み検証済み
- RTF変換関数の実装・検証済み

### Phase 2: 写真整理の自動化（次のステップ）
- 黒板写真のOCR → 工種・測点等の自動読み取り
- folder1〜5 + folderid の自動設定（仕分け）
- picfiletitle, place の自動入力

### Phase 3: 写真帳PDF出力（優先度低）
- PhotoManager自体のPDF出力機能が使えるため
- 独自PDF生成は必要に応じて

### Phase 4: 汎用スキル化
- 水道→舗装・下水・電線共同溝への展開
- 同僚へ共有できるCoworkスキルとして完成

## 備考
- 電子黒板アプリは中小企業の現場では普及していない（スマホ・タブレットが壊れやすいため）
- 入力元は写真に写り込んだ手書き黒板のOCRが前提
- dekigata.dt（出来形データ）は独自バイナリで未解析
