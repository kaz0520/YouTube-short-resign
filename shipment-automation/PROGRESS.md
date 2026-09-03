# 作業ログ

## 2026-09-03
- プロジェクト立ち上げ。ⓘ MCP確認・ロードマップ・Phase 1着手案を提示。
- 決定: OCRはGoogle Vision APIではなくClaude本体の画像読み取りを採用（工程半減・キー不要）。
- 決定: ExcelはGoogle Driveコネクタ（接続済み）で扱う。
- 作成: `shipment-automation/CLAUDE.md`, `PROGRESS.md`, `docs/setup-connectors.md`。
- 方針転換: ユーザーがCoworkで作った既存スキル `advanced-shipping-note-to-excel` を中核採用。
  OCR→構造化→見本準拠Excel生成まで完成済み。当初のGoogle Vision案は不要に。
- ロードマップ更新: Phase1=スキルで実画像1枚を通す / Phase2=製品マスタ辞書 / Phase3=在庫突合。
- 次の一手: 実画像を1枚（`MMDD_配送区分` 名で）アップロード → スキルでExcel生成を検証。

## 2026-09-03（続き）Phase 1 実行
- 入力: 2026/8/17 の伝票画像4枚（ヤマト / 混合(佐川) / 福山×2枚）。
- 生成: `出荷リスト_8月_20260903_001.xlsx`（シート: 0817_福山 / 0817_ヤマト / 0817_混合 / 要確認）。
- 生成器を自作: `scripts/generate_shipping_book.py`（3シート分割・複数製品セル結合・同梱先プルダウン・要確認黄セル・上下中央）。バンドルの単一シート版より見本仕様に忠実。
- 検算: 福山の個口合計=23 → 伝票の「13件23ケ口」と一致。
- 要確認11件（MS/MSQ型式・販売店サンパック・T管/SV型式・混合No.3の佐直引 等）→ 要確認シートに集約。
- 出力xlsxは `.gitignore`（生成物のため）。data/scriptsのみ版管理。
- 次の一手: ユーザーのレビュー（黄セル修正）→ 確定後、製品マスタ辞書(Phase2)と在庫引き落とし(Phase3)へ。
