# Upstream sync 手順

各 skill は外部 upstream の skill ファイル群を fork して、AI エージェント (Managed Agent) 用に汎用化したものを収録している。upstream に新版が出たら、ここに書いた手順で取り込む。

> **⚡ 前提ドキュメント**: fork するときに **何を削って何を残すか** の判定原則は [`GENERALIZATION_GUIDE.md`](./GENERALIZATION_GUIDE.md) に集約している。skill 別の削除ルール (下記 rule B/C) は本 guide の適用結果であり、新しい記述が upstream に入ってきたときは **まず guide の削除必須パターンを sweep** してから rule を更新する。

---

## `content-production`

### Source

- **Origin**: `github.com/jirhigashi-sketch/claude-skills-repo` — `skills/content-production/` (mainline: `origin/main`)
- **Last synced from**: v1.3.4 (2026-08-13, upstream PR **#258** = 予約ページの path を予約語化 / `/reservations/` 使用禁止)
- **Synced at**: 2026-08-13
- **前回**: v1.3.3 (2026-07-24, SHA `21234deb` = Content Collections の Astro 5 glob loader 正典化) / synced at 2026-07-27

> ⚠️ **v1.3.4 は upstream PR #258 と並行で作成した**。本 PR は **upstream #258 のマージ後にマージ**すること（レビューで文言が変わった場合は追随する）。マージ後に上記 `Last synced from` の PR 番号を merge SHA に置き換える。
- **Tracking source**: 上記 git repo (2026-07-03 に zip 配布から切替済)

### 系統ズレメモ (履歴)

初回 sync (2026-06-04) は Downloads 経由の zip `v1.1.0 (2026-06-02)` から取得。これは upstream mainline とは別ラインの手動 export だった。2026-07-03 に mainline (v1.0.7) へ再統合。バージョン番号は v1.1.0 → v1.0.7 と数字上は逆行するが、内容としては mainline に統合済み (Naoki → Producer リネーム、canonical 14 業種、"1 記事 = 1 ファイル" / フロントマター SSoT ルール、非商品化コンテンツ思想などを取り込み)。

### 汎用化のために適用した削除ルール

upstream から取得後、以下を適用して catalog を組み立てる。

#### A. ファイル削除 (案件業務系テンプレ 4 本)

`templates/` 配下から以下を削除:

- `case-consent-form.md` — 事例掲載許諾書 (受注業務)
- `ceo-interview-sheet.md` — 代表インタビュー取材シート (受注業務)
- `image-asset-list.md` — 画像手配リスト (案件管理表)
- `photo-shoot-direction.md` — 撮影指示書 (撮影発注は MA の範囲外)

#### A'. Phase3 テンプレの判定 (v1.0.5 以降で upstream に追加された運用系 5 本)

MA スコープ = **制作時のみ** (公開後の継続運用は対象外) の方針で以下を判定:

- ✅ 採用 (制作品質を担保): `claim-register.template.md` / `content-qa-checklist.template.md`
- ⏸ 保留 (再検討): `content-brief.template.md` — 原稿執筆前に人間が埋める企画書。SKILL.md の「制作フロー」に組み込むかで再検討
- ❌ 不採用 (公開後運用): `content-ops-plan.template.md` / `content-update-report.template.md`

#### B. SKILL.md の編集

| 削除/置換 | 元の場所 | 内容 |
|---|---|---|
| 削除 | `## 運用の標準構成（Producer 運用準拠）` セクション全体 | 案件パス規約 / 外部 docs (`docs/producer-standard-stack.md`) 参照 / sibling skill 参照 (`astro-base-theme`) |
| 抽出 → `## 標準構成` | 上記セクション内の "本スキルでの参照ポイント" (原稿フォーマット + 画像配置) | Astro Content Collections / `public/images/` / 1 記事 1 ファイル / フロントマター SSoT のみ残す (sibling ref は削除) |
| 削除 | `### 姉妹スキルとの使い分け` テーブル + `**境界線の判断**` 箇条書き | sibling skills (`client-intake` / `astro-base-theme` / `frontend-design` / `site-launch-ops` / `site-diagnosis` / `site-audit` / `visual-brief`) は MA 環境に存在しない |
| 削除 | `### プロセス6: 撮影ディレクション` セクション全体 | MA は撮影発注しない |
| 番号繰下げ | `### プロセス7: AI画像生成` → `### プロセス6: AI画像生成` | プロセス 6 削除に伴う |
| 番号繰下げ | `## 実施プロセス（7プロセス）` → `## 実施プロセス（6プロセス）` | 同上 |
| 削除 | `## 成果物の出力先と標準ファイル名` セクション全体 | 案件パス規約 `projects/{project-slug}/04_content/` / CLAUDE.md §2 参照 |
| 削除 | `## 新フロー（層分離）での原稿・画像の位置づけ` セクション全体 | sibling skill 参照 (`visual-brief` 廃止・生成プロンプト STEP1-3 等) / 外部 docs 参照 |
| 置換 | `⚡ このスキルの2つの役割` blockquote | 「A. 初期制作 / B. 公開後運用」の 2 分割は制作寄せで無効。B の記述と関連テンプレ (`content-ops-plan` / `content-update-report`) 言及を削除、思想と採用テンプレ (`claim-register` / `content-qa-checklist`) の 2 本のみに整理 |
| 置換 | `### プロセス1: 前提情報の確認` 本文 | `client-intake の成果物から取得` → `ユーザーの依頼から確認` |
| 置換 | `プロセス5: 画像調達判断` の "4 手段" | 「新規撮影」削除で "3 手段" (既存/買取/AI生成) に |
| 追加 | `プロセス3: 原稿執筆` 末尾 | 執筆プロセスに Step 8 として "公開前 QA (`content-qa-checklist` / `claim-register`)" を追加 |
| 削除 | `## 最初にやること` の "client-intake の成果物を確認" | sibling skill 参照 |
| 置換 | `## 最初にやること` の "撮影指示書 or AI生成プロンプト作成" | "AI生成プロンプト作成" のみに整理 |
| 削除 | `## 最初にやること` の "画像手配リスト更新" | image-asset-list.md 削除に伴う |
| 削除 | `## 最初にやること` の "実装チームへの引き渡し" | sibling skill 参照 |
| 追加 | `## 最初にやること` 末尾 | "納品前の最終チェック" に `content-qa-checklist` / `claim-register` 言及を追加 |
| 削除 | `## 参照ファイル一覧` テーブルから 4 行: photo-shoot-direction / image-asset-list / case-consent-form / ceo-interview-sheet | テンプレ削除に伴う |
| 追加 | `## 参照ファイル一覧` テーブルに 3 行: non-commodity-content-standard / claim-register.template / content-qa-checklist.template | Phase3 採用分と参照 |
| 追加 | `## 禁止事項` に 2 項目 | "根拠のない数値・主張" に claim-register 誘導 / "公開前 QA を通していない原稿の納品" を追加 |
| 置換 | `## 禁止事項` の "client-intake の成果物を確認しない状態での着手" | "依頼内容を確認しない状態での着手" |
| 置換 | 本文中の `astro-base-theme` / `frontend-design` / `docs/*` への参照 | "実装" / 削除 等の単純化 |
| 削除 | 「原稿フォーマット」/ 禁止事項 の実装側正本パス `skills/astro-base-theme/references/part-0-common-spec.md` 0-9 (v1.3.3 で追加) | sibling skill 参照。規範本体 (Astro 5 `glob()` loader 方式 / レガシー記法 `type: 'content'` 禁止 / `.md` 付き URL → 404 の理由) は craft として保持する |
| 置換 | frontmatter `description` | sibling skill (`client-intake` / `site-diagnosis` / `site-audit` / `visual-brief`) 参照を削除、`撮影ディレクション` / `photo direction` / `新規撮影` 言及を削除 |
| 追加 | `## バージョン履歴` 冒頭に fork Note | 「これは MA fork。sibling ref / 撮影 / 案件業務系テンプレ / 継続運用スキームを削除ルール適用」旨を明記 |

#### C. 維持する (= upstream と一致させる)

> **注意**: 「完全一致で維持」は **rule D の sweep を通した結果として差分が出なかった** ファイルにのみ当てはまる。無条件コピーの許可ではない。コピー後は必ず rule D の sweep を回すこと (旧 rule C の無条件維持が事故 #330 の原因)。

- `references/seo-content-strategy.md`: 現時点で完全一致
- `templates/` の共通 3 本 (`page-copy-templates.md` / `seo-article-templates.md` / `seo-annual-balance-sheet.md`): 現時点で完全一致
- `templates/claim-register.template.md` / `content-qa-checklist.template.md` (Phase3 採用): 現時点で完全一致
- `references/non-commodity-content-standard.md` / `templates/ai-image-prompt-library.md`: 採用するが**完全一致ではない** (fork 側編集あり → 下記 D 参照)
- 業種プリセット (`references/industry-presets.md`) は upstream の canonical 14 業種 + multifaceted に統一 (ただし sibling ref は D で削除)
- SKILL.md の `## バージョン履歴` セクション: upstream の歴史的記録として残す (削除された CP-3/CP-4 に対しては fork 側で除外した旨を注記)

#### D. references/ の agent-safe 化 (2026-07-03 追加)

`GENERALIZATION_GUIDE.md` の削除必須パターンに従い、references/ にも削り込みを適用する:

| ファイル | 対処 | 理由 |
|---|---|---|
| `references/image-sourcing-framework.md` | **全面書き換え** (元 445L → 326L) | Unsplash / iStock / Pexels / 写真AC / Pixta を含む買取素材節、カメラマン手配を含む新規撮影節、Dropbox 受け取り workflow、コスト感節を削除し、2 手段 (実行環境のメディアライブラリ / AI 生成) に集約。AI 生成側のツール名 (Midjourney / DALL-E / Firefly / Leonardo) は保持 (craft 知識、実際の tool 実行は system 側の tool が担当)。事故 #330 の再発防止 |
| `references/industry-presets.md` | line 486 の `site-diagnosis 多角化型プリセットの法令クロスチェック表を参照` を汎用化 | sibling skill 参照 |
| `references/writing-process.md` | line 38 / 53 の `client-intake の成果物` を `ユーザーの依頼内容` に置換 | sibling skill 参照 |
| `references/industry-presets.md` | 冒頭の業種コード正本宣言 (`正本 = skills/astro-base-theme/references/industry-presets.md`、upstream v1.0.10 で追加) を除外 | sibling skill 参照。本 fork では本ファイル自身が業種プリセットの正本 |
| `references/non-commodity-content-standard.md` | 末尾の派生文書ガバナンス行 (`docs/derived/` で管理し由来 version を記録、upstream v1.3.1 / FB-07) を除外 | fork に存在しないパス参照 + upstream repo の運用規範であり skill scope 外 |
| `templates/ai-image-prompt-library.md` | 「AI 画像が得意でない領域」の締め `実写撮影かプロのイラストレーターを使用` を、メディアライブラリ参照 + 素材が無ければユーザー依頼で停止、に置換 (2026-07-27 追加) | 人手 workflow (agent 実行不可)。`GENERALIZATION_GUIDE.md` の削除必須パターン該当 |
| `SKILL.md` (rule B と重複するが同系統) | 画像品質基準の `解像度 300dpi 未満` を Web 前提のピクセル基準に置換 (2026-07-27 追加) | 「物理媒体・DPI・印刷」パターン該当。Web 制作 skill に印刷指標は不要 |

**判定基準**: 上記いずれも `GENERALIZATION_GUIDE.md` の「削除必須パターン」に該当。references/ を upstream と完全一致で維持する discipline (旧 rule C) は、agent 幻覚事故 (#330) の原因になったため撤回。以降 references/ にも rule B と同じ deep 削除ルールを適用する。

### 取り込み手順 (チェックリスト)

前提: upstream repo (`claude-skills-repo`) をローカルに clone 済みであること。
標準クローン場所: `/Users/admin/user/projects/fuk/jir/claude-skills-repo` (存在しなければ `git clone git@github.com:jirhigashi-sketch/claude-skills-repo.git` で取得)

0. **⚠ 先に本 repo を最新化する** (これを飛ばすと、既に merge 済みの sync を二重に作業することになる):
   ```bash
   cd /Users/admin/user/projects/fuk/fuk-ai-platform-skills
   git fetch origin && git checkout main && git merge --ff-only origin/main
   ```
   `Last synced from` の SHA は **origin/main の内容** が正。ローカル main が古いまま差分を取ると、upstream 差分を過大に見積もる (実例: 2026-07-27。5 コミット遅れの main を基準にして、PR #1/#2 で取込済みの v1.1.0〜v1.3.2 を未取込と誤認した)。
1. **upstream の最新を取得**:
   ```bash
   cd /Users/admin/user/projects/fuk/jir/claude-skills-repo
   git fetch origin
   ```
   **`git checkout origin/main` はしない**。この clone は別ブランチで作業中・未コミット変更ありの場合がある (実例: 2026-07-27 に `feat/wrangler-v4-node22-template` で staged 変更あり)。以降の参照は全て `git show origin/main:<path>` / `git diff ...origin/main` で read-only に行う。
2. **前回 sync 以降の変更を確認**:
   ```bash
   git log --oneline <last-synced-sha>..origin/main -- skills/content-production/
   git diff <last-synced-sha>..origin/main -- skills/content-production/
   git diff --stat <last-synced-sha>..origin/main -- skills/content-production/   # 変更ファイルの特定
   ```
   → 本ファイル末尾の "Sync 履歴" に前回の SHA を記録している
3. **⚠ 変更のあったファイルだけを選択コピー** (blanket `cp -r references/*` は禁止):
   ```bash
   # diff --stat に出たファイルのみ列挙する。fork 側で全面書き換え済みの
   # references/image-sourcing-framework.md を上書きすると事故 #330 の修正が飛ぶ
   UP=/Users/admin/user/projects/fuk/jir/claude-skills-repo
   DST=/Users/admin/user/projects/fuk/fuk-ai-platform-skills/catalog/content-production
   cd "$UP"
   for f in <上記 diff --stat に出たファイル (fork 収録分のみ)>; do
     git show origin/main:skills/content-production/$f > "$DST/$f"
   done
   ```
   **絶対に上書きしないファイル**: `references/image-sourcing-framework.md` (fork 全面書き換え)
   **fork 非収録 = コピー対象外**: `_design-notes.md` / `evals/` / rule A で削除した templates 4 本 / rule A' 不採用の 3 本
4. **コピーしたファイルに rule D の sweep をかける**:
   ```bash
   cd "$DST" && grep -rnEi "client-intake|astro-base-theme|site-diagnosis|site-audit|visual-brief|site-launch-ops|frontend-design|docs/|projects/\{|Unsplash|Pexels|Dropbox|iStock|カメラマン|撮影|印刷|見積|検収|キックオフ|dpi|イラストレーター" .
   ```
   ヒットを 1 件ずつ判定 (ベンダー名が「禁止」文脈で出るのは OK)。新パターンが出たら rule D の表に追記する。
5. **SKILL.md は手動 3-way merge**:
   - upstream の SKILL.md と本 repo の SKILL.md を diff
   - upstream の本文改善を取り込む
   - 上記 B. の削除/置換ルールを再適用 (sibling skill 参照などが復活していないか確認)
   - `Naoki → Producer` リネームなど、upstream で既に汎用化された変更はそのまま取り込む (削除ルール B の重複部分は不要になる)
6. **本ファイル (`UPSTREAM_SYNC.md`) の "Last synced from" / "Synced at" / 末尾の "Sync 履歴" を更新** (SHA を記録)
7. **取り込み PR を出す**

### Sync 履歴

| Synced at | upstream SHA | upstream version | 備考 |
|---|---|---|---|
| 2026-06-04 | (zip 配布のため N/A) | v1.1.0 | Downloads 経由の別ライン。tracking source 切替前 |
| 2026-07-03 | `a28457cb` | v1.0.7 | tracking source を `claude-skills-repo` git repo に切替。Phase3 テンプレ 5 本のうち `claim-register` / `content-qa-checklist` を採用、`content-brief` は保留、`content-ops-plan` / `content-update-report` は不採用 (制作時のみスコープ)。references/ に `non-commodity-content-standard.md` を追加、旧別ライン由来の `ai-search-content-principles.md` を削除 |
| 2026-07-03 | (fork 内 cleanup) | v1.0.7 | 事故 #330 対応。`GENERALIZATION_GUIDE.md` を新設し、rule D (references/ の agent-safe 化) を追加。`image-sourcing-framework.md` を 2 手段 (メディアライブラリ / AI 生成) に集約 (Unsplash 等の外部 stock ベンダー名指し全削除、Dropbox 等の人手 workflow 削除、コスト感節削除)。`writing-process.md` / `industry-presets.md` の sibling ref (`client-intake` / `site-diagnosis`) を掃除。SKILL.md を "3 手段" → "2 手段" に整合 |
| 2026-07-14 | `18e8e0f4` | v1.1.0 | upstream PR-A 医療法令アップデート (FB-01〜03) を同期。`references/industry-presets.md` (医療広告等ガイドライン名称正典化＋参照正本ブロック / 患者体験談 per-se 禁止化＋BA規律分離 / 外部口コミ転載NG＋ステマ告示) と `templates/content-qa-checklist.template.md` (医療3分割＋外部口コミ/ステマ2項目) に法令デルタを適用。SKILL.md は frontmatter version 1.0.7→1.1.0・description の法令名・鉄則「業種の法令を踏まえる」を新名称へ。全て法令 craft (guide 分類2) のため保持。upstream v1.0.10 の業種コード正本宣言ヘッダ (正本 = astro-base-theme) は sibling ref のため rule D で除外。※ FB-04〜08 は upstream 側で PR-B〜D として順次予定 (本 sync では未取込) |
| 2026-07-16 | `5bde034f` | v1.3.1 | upstream PR-B/C/D (FB-04/05/06/08) を同期。`seo-content-strategy.md` (FAQ 固定レシピ廃止→価値ベース / FAQPage 効果記述全廃＋新規非推奨注記 / HowTo 削除・Review self-serving / Search Gallery 確認) ・`writing-process.md` (FAQ/口コミ行) ・`non-commodity-content-standard.md` (§3 定型 Q&A 量産禁止 / §2 数値8項目参照) ・`claim-register.template.md` (同意状態列＋必須8項目＋法令注意) ・`content-qa-checklist.template.md` (数値項目を3点確認へ) に craft デルタを適用。SKILL.md version 1.1.0→1.3.1。**FB-07 (PR-D) は N/A**: upstream の `docs/skill-feedback-loop.md`＋`docs/derived/` の派生文書ガバナンス基盤で本 fork に該当ファイルが無い。non-commodity 正本宣言の派生文書行も fork 非存在パス (`docs/derived/`) 参照のため rule D で除外。version 番号のみ upstream 1.3.1 に合わせる (FB-07 実体は非取込) |
| 2026-07-27 | `21234deb` | v1.3.3 | upstream の Content Collections Astro 5 正典化を同期。SKILL.md のみ (upstream 差分も SKILL.md 1 ファイル)。「標準構成」の原稿フォーマットを `src/content/config.ts` → `src/content.config.ts` + `glob()` loader 方式へ、禁止事項に項目11 (レガシー記法 `type: 'content'` / `type: 'data'` 前提の納品・実装指定を禁止。理由: Astro 5 では entry `id` に拡張子が残り `.md` 付き URL → 404) を追加。upstream 併記の実装側正本パス (`astro-base-theme/.../part-0-common-spec.md` 0-9) は rule D で除外し規範本体のみ保持。**併せて fork 内の残存 2 件を掃除**: SKILL.md の `300dpi` → Web 前提のピクセル基準、`ai-image-prompt-library.md` の「実写撮影かプロのイラストレーター」→ メディアライブラリ参照 + ユーザー依頼で停止 (いずれも旧 rule C の無条件維持で残っていた分)。取り込み手順に step 0 (本 repo の最新化) を新設し、step 1/3 を read-only 参照・選択コピーに改訂 |
| 2026-07-17 | `5119a937` | v1.3.2 | upstream の名称統一追い掃除 (旧称「医療広告ガイドライン」→「医療広告等ガイドライン」) を同期。`references/writing-process.md` / `templates/ai-image-prompt-library.md` の各1箇所。名称のみで挙動不変。upstream で対象の `case-consent-form.md` は fork で削除済 (rule A)、`_design-notes.md` / `evals/` は fork 非収録のため対象外。SKILL.md version 1.3.1→1.3.2 |

### システム側 (consumer) への反映

取り込み PR を main に merge した後、consumer 側 (fuk-ai-platform 等) で:

```bash
pnpm update agent-skills-catalog
pnpm skills:sync --tenant <slug> --force
```

を実行して Anthropic Managed Agents に新版を upload する。
