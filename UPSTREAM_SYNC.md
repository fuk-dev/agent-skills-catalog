# Upstream sync 手順

各 skill は外部 upstream の skill ファイル群を fork して、AI エージェント (Managed Agent) 用に汎用化したものを収録している。upstream に新版が出たら、ここに書いた手順で取り込む。

---

## `content-production`

### Source

- **Origin**: HP制作者 (Naoki) の `content-production` skill
- **Last synced from**: v1.1.0 (2026-06-02)
- **Synced at**: 2026-06-04

### 汎用化のために適用した削除ルール

upstream の zip を展開後、catalog にコピーする前 (or 後) に以下を適用する。

#### A. ファイル削除 (案件業務系テンプレ 4 本)

`templates/` 配下から以下を削除:

- `case-consent-form.md` — 事例掲載許諾書 (受注業務)
- `ceo-interview-sheet.md` — 代表インタビュー取材シート (受注業務)
- `image-asset-list.md` — 画像手配リスト (案件管理表)
- `photo-shoot-direction.md` — 撮影指示書 (撮影発注は MA の範囲外)

#### B. SKILL.md の編集

| 削除/置換 | 元の場所 | 内容 |
|---|---|---|
| 削除 | `## 運用の標準構成（Naoki 運用準拠）` セクション全体 | Naoki 個人の運用前提 |
| 削除 | `### 姉妹スキルとの使い分け` テーブル + `**境界線の判断**` 箇条書き | sibling skills (`client-intake` / `astro-base-theme` / `frontend-design` / `site-launch-ops` / `site-diagnosis` / `site-audit`) は MA 環境に存在しない |
| 削除 | `### プロセス6: 撮影ディレクション` セクション全体 | MA は撮影発注しない |
| 番号繰下げ | `### プロセス7: AI画像生成` → `### プロセス6: AI画像生成` | プロセス 6 削除に伴う |
| 番号繰下げ | `## 実施プロセス（7プロセス）` → `## 実施プロセス（6プロセス）` | 同上 |
| 置換 | `### プロセス1: 前提情報の確認` 本文 | `client-intake の成果物から取得` → `ユーザーの依頼から確認` |
| 削除 | `## 最初にやること` の Step 1 (`client-intake の成果物を確認`) | sibling skill 参照 |
| 削除 | `## 最初にやること` の Step 8 (`撮影指示書 or AI生成プロンプト作成`) → 「AI生成プロンプト作成」のみに整理 | photo-shoot-direction.md 削除 |
| 削除 | `## 最初にやること` の Step 9 (`画像手配リスト更新`) | image-asset-list.md 削除 |
| 削除 | `## 最初にやること` の Step 11 (`実装チームへの引き渡し`) | sibling skill 参照 |
| 番号繰り上げ | `## 最初にやること` の残り steps | 上記削除に伴う |
| 削除 | `## 参照ファイル一覧` テーブルから 4 行: photo-shoot-direction / image-asset-list / case-consent-form / ceo-interview-sheet | テンプレ削除に伴う |
| 置換 | `## 禁止事項` の "client-intake の成果物を確認しない状態での着手" | "依頼内容を確認しない状態での着手" |
| 置換 | 本文中の `astro-base-theme` / `frontend-design` への参照 | "実装" / 削除 等の単純化 |
| 置換 | frontmatter `description` | sibling skill (`client-intake` / `site-diagnosis` / `site-audit`) への参照を削除、`撮影ディレクション` 言及を削除 |

#### C. 維持する (= upstream と一致させる)

- `references/*` 全 5 本: 完全一致
- `templates/` の残り 4 本 (`page-copy-templates.md` / `seo-article-templates.md` / `ai-image-prompt-library.md` / `seo-annual-balance-sheet.md`): 完全一致
- 業種プリセット (`references/industry-presets.md`) は 8 業種のまま (LLM が読む業種別ガイドラインで、システム側 registry の IndustryKey とは別レイヤー)
- SKILL.md の `## バージョン履歴` セクション: upstream の歴史的記録として残す

### 取り込み手順 (チェックリスト)

1. **upstream の新 zip を入手** → `/tmp/upstream-content-production/` に展開
2. **references/ を上書きコピー**:
   ```bash
   cp -r /tmp/upstream-content-production/content-production/references/* \
         catalog/content-production/references/
   ```
3. **templates/ の "残す 4 本" だけ上書きコピー**:
   ```bash
   for f in page-copy-templates.md seo-article-templates.md ai-image-prompt-library.md seo-annual-balance-sheet.md; do
     cp /tmp/upstream-content-production/content-production/templates/$f \
        catalog/content-production/templates/$f
   done
   ```
4. **SKILL.md は手動 3-way merge**:
   - upstream の新 SKILL.md と本 repo の SKILL.md を diff
   - upstream の本文改善を取り込む
   - 上記 B. の削除/置換ルールを再適用 (sibling skill 参照などが復活していないか確認)
5. **本ファイル (`UPSTREAM_SYNC.md`) の "Last synced from" / "Synced at" を更新**
6. **取り込み PR を出す**

### システム側 (consumer) への反映

取り込み PR を main に merge した後、consumer 側 (fuk-ai-platform 等) で:

```bash
pnpm update agent-skills-catalog
pnpm skills:sync --tenant <slug> --force
```

を実行して Anthropic Managed Agents に新版を upload する。
