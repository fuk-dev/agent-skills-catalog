# 汎用スキル化ガイド

本 repo (agent-skills-catalog) は、外部で生まれた **人間向けの制作スキル** を fork し、**Managed Agent (MA) 用の汎用スキル** に整えて収録するためのカタログです。

このガイドは、fork 元をどう削り・どう残すかの **判定原則** をまとめたものです。skill 別の削除ルールは各 skill の `UPSTREAM_SYNC.md` (or 該当 skill の同名セクション) に書きますが、その削除ルールを立てるときの **共通の物差し** は本ガイドを見てください。

---

## 判定 3 分類

skill に書かれている記述を、以下 3 分類のどれかに当てはめる。

### 1. 人間向け (削除対象)

- agent が物理的に実行できない workflow (人手調達・撮影・購入・打ち合わせ・郵送・電話)
- 実行環境に存在しない外部サービスの **名指し推奨** (Unsplash / iStock / Dropbox / Slack / Notion 等)
- 人間の裁量前提の運用 (「クライアントに確認する」「担当者判断」「MTG で決める」)
- 物理媒体・案件業務 (印刷・DPI・紙媒体・見積・検収・キックオフ)

→ **削除する**。system prompt override で無効化するのは信頼性が低い (プロンプトインジェクション耐性が保証されない・長 context で薄まる)。**source of truth 側から抜く** のが正解。

### 2. agent 実行可 craft (保持対象)

- プロンプト設計・ライティング原則・SEO 戦略・コンテンツ品質論
- 法令チェック (医療広告 GL・薬機法・景表法・宅建業法 等)
- 品質基準 (解像度・フォーマット・ファイル名規約 等)
- 業種別トーン・NG 表現・必須記載

→ そのまま保持。agent が自分で判断・生成に使える。

### 3. 汎用原則 (保持対象)

- 「AI は補助、価値は一次情報」等の思想
- 業種プリセット・ライティング原則・鉄則
- コモディティ vs 独自情報 の判断軸

→ そのまま保持。人間向けでも agent 向けでも通用する。

---

## 削除必須パターン (agent 幻覚を誘発する)

過去に事故が起きた or 起きそうなパターン。**該当したら削除**:

| パターン | 例 | なぜ削除するか |
|---|---|---|
| 具体ベンダー名の列挙 (実行環境に無いもの) | `無料 → Unsplash / Pexels / 写真AC` | agent が `images.unsplash.com/...` を書いて存在しない URL 参照 (実例: #330) |
| 「A を使え / A で受け取る」形式の推奨 | `クライアントから DropBox で素材提供を受ける` | agent は Dropbox にアクセスできない、幻覚 or フリーズ |
| 人手 workflow の詳細 | `カメラマンを手配`, `MTG で決定`, `印刷入稿` | agent 実行不可 |
| 物理媒体・DPI・印刷 | `300dpi 以上で印刷入稿` | agent 環境に紙媒体は無い |
| 案件業務系 | `見積提出`, `検収`, `キックオフ MTG` | 事業運営の話、skill scope 外 |
| sibling skill 参照 (MA に存在しないスキル名) | `client-intake の成果物を受け取る` | 存在しない skill を待って停止 |
| 外部 docs への path 参照 | `docs/producer-standard-stack.md` | fork 環境に無いパス、404 相当 |
| 案件固有パス規約 | `projects/{project-slug}/04_content/` | 実行環境と規約が違うと agent が誤ったパスに出力 |

---

## リフレーム原則 (削除しきれない場合)

削除すると craft 知識まで失う場合は、**具体ベンダー名や人手前提を抽象化** して残す:

| Before (問題あり) | After (汎用化) |
|---|---|
| `Unsplash / Pexels から取得` | `実行環境が提供するメディアライブラリから選択` |
| `Dropbox で受け取る` | `環境が提供するメディアライブラリを参照する` |
| `カメラマンに撮影依頼` | (削除。撮影は agent scope 外) |
| `client-intake の成果物を確認` | `ユーザーの依頼内容を確認` |
| `astro-base-theme の実装に引き渡す` | `実装がそのまま使える形式で納品` |

---

## 削除判定チェックリスト

新規 skill を fork するとき、および upstream sync で新しい記述が入ってきたとき、以下を確認:

- [ ] この記述を読んで agent が忠実に従うと、実在しない URL / 存在しないツール / 実行不可な action を出力する可能性があるか?
- [ ] system prompt override で完全に無効化できるか? (できないなら削除必須)
- [ ] 削除後に craft 知識 (何が良いか) は残るか? (残らないなら §リフレーム原則 で抽象化して保持)
- [ ] MA に存在しない sibling skill 名を含んでいるか? (含んでいたら削除・置換)
- [ ] 特定の外部サービス名を **推奨する形** で列挙しているか? (列挙 = agent 幻覚の元凶)

---

## upstream sync 時の適用フロー

1. upstream の差分を取得 (`git diff <last-synced-sha>..origin/main -- <skill-path>/`)
2. 差分ファイルごとに本ガイドの **削除必須パターン sweep** を実行:
   ```bash
   grep -nEi "Unsplash|Pexels|Dropbox|iStock|Adobe Stock|Shutterstock|カメラマン|印刷|見積|検収|キックオフ" <skill>/
   grep -nE "<sibling skill 名>" <skill>/
   ```
3. 該当箇所を削除 or §リフレーム原則 に沿って抽象化
4. skill 別の `UPSTREAM_SYNC.md` の削除ルールに **新パターンが出た場合は追記**
5. Sync 履歴に SHA を記録
6. PR 提出

---

## 過去の事故例 (learning)

新しい事故を検知したら、原因と対策をここに追記する。

### 2026-07-03 · content-production の Unsplash 名指しで URL 幻覚 (#330)

- **事象**: MA agent が `images.unsplash.com/photo-*` の存在しない URL を出力
- **原因**: `references/image-sourcing-framework.md` が `無料 → Unsplash / Pexels / 写真AC` と明示推奨していた
- **対応**: image-sourcing-framework.md を 2 手段 (実行環境のメディアライブラリ / AI 生成) に集約、買取素材節・新規撮影節を削除、Dropbox 等の人手 workflow を汎用化
- **教訓**: `UPSTREAM_SYNC.md` の rule C (`references/* 完全一致で維持`) の甘さが露呈。references/ にも本ガイドの削除必須パターンを適用する discipline に変更
