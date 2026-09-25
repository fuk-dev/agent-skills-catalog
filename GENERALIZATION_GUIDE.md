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
| 実行環境に無い計測基盤の前提 | `npx playwright install chromium webkit` して実描画を走査 | MA sandbox に無い依存を skill が主動線に置くと、agent は実行できず停止するか、測っていないものを「0 件」と報告する |

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

## 部分抽出の原則 (skill 単位で採否を決めない)

upstream の skill は **1 本の中に複数の性質が同居している**ことがある。典型は検査・監査系で、
**予防 (生成時に守る規約) / 検出 (実行環境依存の計測) / 往復の進め方 (人間とのやりとり)** が
1 skill にまとまっている。ローカルの Claude Code では 1 本で完結できるので、upstream 側の設計としては
正しい。しかし MA には計測基盤が無いため、**skill 単位で「採用」「不採用」を決めると、
本来使える craft ごと捨てることになる**。

### 判定の順序

1. **skill の主動線が実行環境依存か** を見る (Playwright・外部 package・専用 CLI 等)
2. 依存する場合、**その skill の中に依存しない部分があるか** を探す
   - `references/` がファイル単位で分離されていれば、そこが抽出単位になる
   - 「生成時に守れば発生しない」「原則集」「規約」といった節は依存しないことが多い
3. 抽出する場合、**残す側から参照されている非収録ファイルを潰す** (rule D)
   - 存在しないパスへの参照は、それ自体が削除必須パターン
4. **SKILL.md は流用せず fork 側で書き下ろす**
   - upstream の SKILL.md は検出フローを主動線にしているため、description も本文も方針が合わない
   - `description` は MA の auto-invoke を決めるので、抽出後のスコープで書き直す必要がある

### 記録の義務

不採用にした skill も、**判定と理由を `UPSTREAM_SYNC.md` の「新規 skill の採用判定」に残す**。
残さないと次回 sync で同じ調査を繰り返す。browser 非依存の craft を内部に持っていた場合は、
**将来の採用候補として明記する** (例: `design-audit/references/design-principles.md` の原則集)。

### 適用例

- **2026-09-25 `jp-typography`**: 検出 (`detect.md`) を捨て、予防 (`rules.md`) と実装レシピ
  (`fix-patterns.md`) を採用。SKILL.md は書き下ろし。upstream が予防と検出をファイル単位で
  分離していたため抽出が容易だった
- **2026-09-25 `hp-audit` / `design-audit`**: 主動線が実描画計測で、かつ内部の craft も
  人手運用 (waiver・案件台帳・キャリブレーション) と絡んでいたため不採用。`design-principles.md`
  のみ将来候補として記録


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
