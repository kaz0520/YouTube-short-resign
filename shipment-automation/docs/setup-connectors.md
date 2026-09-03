# ⓪ MCP / コネクタ確認（詳細）

手書き出荷リスト → デジタル化 → Excel管理 のフローで使う連携の判定結果。

## このフローに関係する連携

| 用途 | フローの当初案 | 2026年の推奨 | 理由 |
|---|---|---|---|
| 手書き画像 → テキスト抽出（OCR） | Google Vision API | **Claude本体の画像読み取り**（コネクタ不要） | 手書きでも文字誤り率 約2%。APIキー取得・課金設定ゼロで即使える。客先/商品/個数の「意味」まで一発で構造化できるのが最大の利点 |
| Excel（在庫・客先）の読み書き | GWS（Google Workspace） | **Google Drive コネクタ（接続済み）** | 表計算ファイルの検索・読み・アップロードに対応。ファイルが Google スプレッドシートでも .xlsx でも扱える |
| 一時保存（CSV/JSON） | folder | **このプロジェクトフォルダ** | Git管理下に置けば履歴も残る |

## 接続状況（このユーザー環境）

接続済みで、このチャットで即使えるコネクタ:
- Gmail
- Google Calendar
- **Google Drive** ← 本フローで使用
- Notion
- Supabase

未接続で追加が必要なもの: **なし**

## なぜ Google Vision API を使わないのか（初心者向け補足）

- Google Vision API =「画像から文字だけを抜き出す」道具。抜き出した後、「どれが客先でどれが個数か」は別途プログラムで判定する必要がある
- Claude本体の画像読み取り =「画像を見て、客先・商品・個数を最初から表として理解する」。OCRと解析が1ステップになるので、初心者にとって工程が半分で済む
- 精度も手書きで実用レベル（誤りは人間レビューで直す前提のフロー）

## 参考（2026年 OCR比較）

- 手書きOCRはLLM系が実用段階。Claude Vision の文字誤り率(CER)は約2.1%（印刷文字）。
- Sources:
  - https://aimultiple.com/ocr-accuracy
  - https://www.codesota.com/ocr/best-for-handwriting
  - https://www.analyticsinsight.net/artificial-intelligence/best-computer-vision-apis-and-ai-models-in-2026
