# agent-skills-catalog

AI エージェント (Anthropic Managed Agents 形式) で使う skill カタログ。サイトの原稿執筆・画像配置などを体系化した skill を収録する。

## 含まれる skill

| skill | 説明 |
|---|---|
| [`content-production`](catalog/content-production/SKILL.md) | サイトの原稿執筆・画像配置 (TOP/下層/事例/FAQ/SEO 記事) |

## 使い方 (git dependency として取り込む)

`package.json` (`<owner>` は本 repo の GitHub owner に置き換える):

```json
{
  "dependencies": {
    "agent-skills-catalog": "github:<owner>/agent-skills-catalog#main"
  }
}
```

その後、利用側の loader で `node_modules/agent-skills-catalog/catalog/<name>/SKILL.md` を読み込んで Anthropic Skills API に upload する。

## skill ファイル形式

各 skill は Anthropic Skills の慣例に従う:

```
catalog/<name>/
├── SKILL.md           # frontmatter (name / description) + 本文
├── references/        # 補助 md (本文から参照される、Level 3)
└── templates/         # テンプレ md (本文から参照される、Level 3)
```

SKILL.md frontmatter:

- `name`: ディレクトリ名と一致する必要あり (`[a-z0-9-]{1,64}`)
- `description`: いつ使うか を明示 (~1024 字以内)

## upstream からの更新の取り込み

各 skill は外部 source からの mirror。upstream に新版が出た時の取り込み手順は [`UPSTREAM_SYNC.md`](UPSTREAM_SYNC.md) を参照。
