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
