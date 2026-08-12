# ショート視聴レコーダー — 運用メモ（Claude向け）

このリポジトリは、YouTubeショートの視聴を「無意識」から「意識的な選択」へ変えるための
個人用アプリと、そのチャット記録運用のためのものです。

## アプリ本体
- `index.html` … 単体で動くWebアプリ（GitHub Pagesで公開、PWAでスマホにインストール可）
- 視聴の開始/終了・アンケート（良かった/普通/後悔した）・履歴（日別回数/合計時間/後悔率）
- ローカル(localStorage)保存＋Supabaseへ自動同期（合言葉=space_key方式）

## チャット記録の運用（ユーザーの主な使い方）
ユーザーは「**両方に保存して**」と送ることで、視聴記録を **Supabase と Notion の両方** に保存する。
毎回、保存後に**フィードバック**（確認＋やさしい振り返り）を返す。

### 保存の手順（Claudeがやること）
1. ユーザーのメッセージから 視聴時間 / 気持ち / 日時 を読み取る
   - 時間や気持ちが不明なときは短く聞き返す
   - 気持ちは good（良かった）/ normal（普通）/ regret（後悔した）に振り分け
   - 日時の指定がなければ「終了=今」「開始=今-視聴時間」とみなす
2. **Supabaseに保存**: `public.short_add_session` RPC を使う（またはテーブルへinsert）
   - space_key は Supabaseの非公開テーブル `public.short_recorder_config`（key='chat_space_key'）から取得する
     （※このキーは秘密。公開リポジトリには絶対に書かない）
   - id はクライアント側と衝突しないよう `gen_random_uuid()` を使う
3. **Notionに保存**: 下記データベースに1行追加（notion-create-pages）
   - 記録ID（Supabaseと同じid）/ 日付 / 開始 / 終了 / 視聴時間（分）/ 気持ち
   - 重複を避けるため、同じ記録IDが既にあれば追加しない
4. **フィードバックを返す**
   - 保存内容の確認（日付・視聴時間・気持ち）
   - かんたんな振り返り（今日の回数・合計時間・最近の後悔率など）
   - 禁止・説教はしない。事実と気づき＋「次にどう選ぶか」に目を向ける一言

### 保存先の識別子
- Supabase project ref: `yqlbidxvkvzyhvjgiohl`（リージョン ap-northeast-1 / JST）
- Supabase table: `public.short_viewing_sessions`（合言葉スコープ、直接アクセス不可・RPC経由）
- Supabase RPC: `short_add_session` / `short_get_sessions` / `short_clear_sessions` / `short_delete_session`
- Notion database: 「ショート視聴レコーダー 履歴」
  - URL: https://app.notion.com/p/cd938f45cf284ec498dfb58a8279e8ab
  - database_id: cd938f45-cf28-4ec4-98df-b58a8279e8ab
  - data source: collection://0331d458-fa3d-4c1b-a96d-ab458dcf61dc

### 時刻の扱い
- ユーザーは日本(JST)。日付・開始/終了の表示はJSTで扱う。
- Supabaseには epoch(ms) で渡す（RPCの p_start_ms / p_end_ms）。

## メモ
- アプリ（スマホ）とチャット記録を同じ履歴にそろえたい場合は、アプリの「合言葉の設定」に
  `short_recorder_config.chat_space_key` の値を入れてもらう（任意）。
