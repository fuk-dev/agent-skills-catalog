# Upstream sync 手順

各 skill は外部 upstream の skill ファイル群を fork して、AI エージェント (Managed Agent) 用に汎用化したものを収録している。upstream に新版が出たら、ここに書いた手順で取り込む。

---

## `content-production`

### Source

- **Origin**: `github.com/jirhigashi-sketch/claude-skills-repo` — `skills/content-production/` (mainline: `origin/main`)
- **Last synced from**: v1.0.7 (2026-06-26, SHA `a28457cb`)
- **Synced at**: 2026-07-03
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
| 置換 | frontmatter `description` | sibling skill (`client-intake` / `site-diagnosis` / `site-audit` / `visual-brief`) 参照を削除、`撮影ディレクション` / `photo direction` / `新規撮影` 言及を削除 |
| 追加 | `## バージョン履歴` 冒頭に fork Note | 「これは MA fork。sibling ref / 撮影 / 案件業務系テンプレ / 継続運用スキームを削除ルール適用」旨を明記 |

#### C. 維持する (= upstream と一致させる)

- `references/*` の共通 4 本 (`image-sourcing-framework.md` / `seo-content-strategy.md` / `writing-process.md` / `industry-presets.md`): 基本 upstream と一致。ただし `industry-presets.md` 冒頭の canonical 14 業種テーブル追加部から `astro-base-theme` sibling ref は削除
- `references/non-commodity-content-standard.md`: Phase3 採用、upstream からそのままコピー
- `templates/` の共通 4 本 (`page-copy-templates.md` / `seo-article-templates.md` / `ai-image-prompt-library.md` / `seo-annual-balance-sheet.md`): 完全一致
- `templates/claim-register.template.md` / `content-qa-checklist.template.md` (Phase3 採用): upstream からそのままコピー
- 業種プリセット (`references/industry-presets.md`) は upstream の canonical 14 業種 + multifaceted に統一
- SKILL.md の `## バージョン履歴` セクション: upstream の歴史的記録として残す (削除された CP-3/CP-4 に対しては fork 側で除外した旨を注記)

### 取り込み手順 (チェックリスト)

前提: upstream repo (`claude-skills-repo`) をローカルに clone 済みであること。
標準クローン場所: `/Users/admin/user/projects/fuk/jir/claude-skills-repo` (存在しなければ `git clone git@github.com:jirhigashi-sketch/claude-skills-repo.git` で取得)

1. **upstream の最新を取得**:
   ```bash
   cd /Users/admin/user/projects/fuk/jir/claude-skills-repo
   git fetch origin
   git checkout origin/main
   ```
2. **前回 sync 以降の変更を確認**:
   ```bash
   git log --oneline <last-synced-sha>..origin/main -- skills/content-production/
   git diff <last-synced-sha>..origin/main -- skills/content-production/
   ```
   → 本ファイル末尾の "Sync 履歴" に前回の SHA を記録している
3. **references/ を上書きコピー**:
   ```bash
   cp -r skills/content-production/references/* \
         /Users/admin/user/projects/fuk/fuk-ai-platform-skills/catalog/content-production/references/
   ```
4. **templates/ の "残す 4 本" だけ上書きコピー**:
   ```bash
   for f in page-copy-templates.md seo-article-templates.md ai-image-prompt-library.md seo-annual-balance-sheet.md; do
     cp skills/content-production/templates/$f \
        /Users/admin/user/projects/fuk/fuk-ai-platform-skills/catalog/content-production/templates/$f
   done
   ```
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

### システム側 (consumer) への反映

取り込み PR を main に merge した後、consumer 側 (fuk-ai-platform 等) で:

```bash
pnpm update agent-skills-catalog
pnpm skills:sync --tenant <slug> --force
```

を実行して Anthropic Managed Agents に新版を upload する。
