# AI 画像生成プロンプトライブラリ

> プロセス7で使用。抽象イメージ・背景・アイコン等の AI 生成時のプロンプト集。
> 実物が必要な場面（施工事例・スタッフ・店舗等）では使用禁止。

---

## 1. このライブラリの使い方

1. 用途に合ったプロンプトを選ぶ
2. 業種・サイトトーンに合わせてカスタマイズ
3. AI 画像生成ツール（Midjourney / DALL-E / Stable Diffusion / Adobe Firefly）で生成
4. 品質チェック（歪み・不自然さ）
5. 商用利用可のプランで生成したか確認

---

## 2. プロンプトの基本構造

```
[主被写体の説明] + [スタイル] + [色調・照明] + [構図] + [品質パラメータ]
```

### 例

```
A modern office interior with warm wooden accents, 
soft natural lighting from large windows, 
minimalist style with beige and white color palette, 
photography style, shot from eye level, 
high resolution, professional, 8k, detailed
```

---

## 3. 日本語プロンプトのコツ

日本語プロンプトは AI によって対応レベルが異なる。以下の工夫：

1. **英語併記**: 「書道の筆 (calligraphy brush)」
2. **シンプルに**: 長すぎず、キーワード中心
3. **日本的要素**: 「和紙」「畳」「和室」「京都」等

---

## 4. 業種別プロンプトテンプレート

### リフォーム業

#### ヒーロー画像（外壁塗装）

```
A beautifully painted traditional Japanese house exterior, 
freshly renovated, modern but warm atmosphere, 
natural daylight, bright blue sky background, 
photography style, professional, 
vibrant colors, high detail, 
aspect ratio 16:9, 8k quality
```

#### サービスイメージ（屋根塗装）

```
A craftsman working on a residential rooftop, 
painting with care and precision, 
bright sunny day, clear blue sky, 
professional photography style, 
focused on hands and tools, 
Japanese residential area in background, 
16:9 aspect ratio, 4k quality
```

#### 抽象的な強み表現

```
Abstract illustration of reliability and craftsmanship, 
minimalist flat design style, 
warm color palette: navy blue, ivory, and subtle orange, 
Japanese design sensibility, 
professional business context, 
16:9 aspect ratio
```

---

### クリニック

#### 待合室イメージ

```
A serene medical clinic waiting room, 
modern interior with soft pastel colors, 
natural light streaming through windows, 
clean minimalist design, comfortable seating, 
welcoming atmosphere, 
photography style, 
no people visible, 
16:9 aspect ratio, high resolution
```

#### 歯科医療の抽象イメージ

```
Abstract illustration representing dental care, 
clean modern vector style, 
soft blue and white gradient, 
subtle tooth motif, 
professional medical feel, 
Japanese design aesthetic, 
centered composition, 
1:1 square aspect ratio
```

#### 美容医療のイメージ

```
Soft, elegant abstract visual for beauty medicine, 
ethereal mist and light petals, 
gentle pink and cream tones, 
feminine and luxurious atmosphere, 
no faces visible, 
16:9 aspect ratio, high resolution
```

**注意**: 医療系は**効果効能を誤認させる画像**は避ける（薬機法リスク）。

---

### 士業

#### 信頼感のある抽象イメージ

```
Abstract professional imagery representing trust and legal expertise, 
clean minimalist design, 
navy blue and gold accent colors, 
geometric patterns, 
Japanese corporate aesthetic, 
subtle and refined, 
16:9 aspect ratio
```

#### 書類・契約書の抽象

```
Neatly arranged documents on a wooden desk, 
soft natural lighting, 
pen and glasses placed thoughtfully, 
Japanese office setting, 
warm amber tones, 
top-down view, 
photography style, 
16:9 aspect ratio
```

---

### 工務店（注文住宅）

#### 家づくりのビジョン

```
A modern Japanese house with warm wooden accents, 
blending traditional and contemporary design, 
surrounded by lush green garden, 
golden hour lighting, 
family-friendly atmosphere, 
photography style, 
16:9 aspect ratio, high quality
```

#### 建築図面のイメージ

```
Architectural blueprint for a modern Japanese home, 
detailed floor plan with measurements, 
professional drafting style, 
blue and white color scheme, 
clean and organized layout, 
top-down view, 
16:9 aspect ratio
```

---

### 広告代理店

#### デジタルマーケティングのビジュアル

```
Abstract visualization of digital marketing data, 
flowing lines and charts, 
dynamic composition, 
modern tech aesthetic, 
blue and orange color palette, 
professional business context, 
16:9 aspect ratio, high detail
```

#### チームワークの抽象

```
Abstract illustration of creative collaboration, 
geometric shapes connecting, 
vibrant but professional colors, 
Japanese design sensibility, 
modern corporate feel, 
16:9 aspect ratio
```

---

### 飲食店

#### 料理写真の雰囲気カット

```
Overhead view of a beautifully plated Japanese dinner, 
fresh ingredients, warm restaurant lighting, 
rustic wooden table with ceramic bowls, 
food photography style, 
appetizing and inviting, 
16:9 aspect ratio, 8k quality
```

**注意**: 実物メニューの代わりに使わない。雰囲気カットのみ。

#### 店内の雰囲気

```
A cozy Japanese izakaya interior, 
warm wooden finishes, 
soft pendant lights creating ambiance, 
empty but welcoming, 
photography style, 
golden and warm tones, 
16:9 aspect ratio
```

---

### 美容室

#### 美容空間の抽象

```
A luxurious hair salon interior, 
modern minimalist design with mirror and plants, 
soft natural lighting, 
neutral color palette: beige, white, terracotta, 
Japanese design aesthetic, 
no people visible, 
photography style, 
16:9 aspect ratio
```

---

### 不動産

#### エリアのイメージ

```
A residential neighborhood in Fukuoka, Japan, 
quiet street with modern houses, 
lush green trees lining the street, 
sunny afternoon, 
aerial or street-level view, 
photography style, 
Japanese suburban aesthetic, 
16:9 aspect ratio, high resolution
```

**注意**: 実在物件の代替として使わない。

---

### ホテル

#### ホテルのラグジュアリーなイメージ

```
A luxurious Japanese ryokan room, 
tatami floor, shoji screens, 
low table with tea set, 
view of a serene garden, 
warm evening light, 
photography style, 
elegant and refined atmosphere, 
16:9 aspect ratio, high quality
```

---

## 5. 用途別プロンプトテンプレート

### OGP 画像

OGP 画像は 1200x630 px、シェア時に目立つ必要がある：

```
Professional OGP image for website, 
clean modern design with text space on the right, 
Japanese corporate aesthetic, 
[業種] themed imagery on left, 
brand colors: [カラー], 
high contrast for readability, 
16:9 aspect ratio (1200x630)
```

### アイコン・シンボル

```
Minimalist icon representing [concept], 
flat design style, 
single color or two tone, 
clean lines, modern aesthetic, 
centered composition, 
white background, 
square aspect ratio (1:1)
```

### 背景・パターン

```
Subtle background pattern, 
geometric and modern, 
very light tones, 
suitable for text overlay, 
Japanese-inspired design, 
seamless tileable pattern, 
square aspect ratio
```

### ヒーロー画像（汎用）

```
A bright and inviting scene for [業種],
natural lighting, warm atmosphere,
professional photography style,
composition with space for text overlay on one side,
brand-appropriate color palette,
wide aspect ratio (16:9 or 21:9),
high resolution, 8k quality
```

---

## 6. スタイル指定のバリエーション

### フォトリアル

```
photography style, professional, high resolution, 8k, 
natural lighting, shot with Canon EOS R5, 50mm lens
```

### イラスト・フラットデザイン

```
flat illustration, minimalist, vector style, 
clean lines, solid colors, modern design
```

### 水彩画風

```
watercolor painting style, soft and gentle, 
artistic brush strokes, pastel colors
```

### 日本画風

```
traditional Japanese ink painting style, 
minimalist, elegant brush strokes, 
muted colors with gold accents
```

### 3D レンダリング

```
3D rendering, realistic, 
professional product visualization, 
soft lighting, detailed textures
```

---

## 7. 色調・ムードの指定

### 明るく親しみやすい

```
bright and friendly atmosphere, 
warm pastel colors, soft lighting, 
inviting and welcoming
```

### 高級感・ラグジュアリー

```
luxurious and sophisticated, 
dark tones with gold accents, 
dramatic lighting, elegant composition
```

### 誠実・プロフェッショナル

```
professional and trustworthy, 
navy blue and white palette, 
clean composition, 
corporate aesthetic
```

### モダン・革新的

```
modern and innovative, 
minimalist design, 
bold accent colors, 
futuristic feel
```

### ナチュラル・自然派

```
natural and organic, 
earthy tones: sage, beige, terracotta, 
soft natural lighting, 
botanical elements
```

---

## 8. 構図の指定

| 英語表現 | 意味 |
|---|---|
| `centered composition` | 中央に被写体 |
| `rule of thirds` | 三分割法 |
| `top-down view` | 真上から |
| `low angle shot` | 下から見上げる |
| `high angle shot` | 上から見下ろす |
| `close-up` | クローズアップ |
| `wide shot` | 広角・引き |
| `medium shot` | 中間距離 |
| `portrait` | ポートレート |
| `landscape` | 風景 |

### アスペクト比

| 比率 | 用途 |
|---|---|
| `1:1` | アイコン、SNS 投稿 |
| `16:9` | Web ヒーロー、YouTube サムネ |
| `9:16` | スマホ縦画面、Stories |
| `4:3` | ブログアイキャッチ |
| `21:9` | ウルトラワイド・ヒーロー |

---

## 9. ネガティブプロンプト（避けたい要素）

不要な要素を除外：

```
--no [避けたい要素]

例:
--no text, watermark, logo, signature
--no people, faces, hands
--no blur, low quality, distortion
```

AI によって記法が異なる：
- Midjourney: `--no text, watermark`
- Stable Diffusion: `Negative prompt: text, watermark, low quality`

---

## 10. 品質保証のチェックリスト

生成した AI 画像は、以下を確認：

```
【技術品質】
[ ] 解像度が 1920px 幅以上あるか
[ ] ノイズ・不自然な圧縮がないか
[ ] 色調がバランス良いか

【構図】
[ ] 意図した構図になっているか
[ ] 被写体が中心または意図した位置か
[ ] 余白が適切か

【不自然な要素の有無】
[ ] 手・指の数や形が正常か
[ ] 顔のパーツが自然か（目・鼻・口）
[ ] 背景に意味不明な文字・記号がないか
[ ] 物体の歪み・溶けがないか

【ブランド一致】
[ ] ブランドカラーに合致するか
[ ] 業種のトーンに合うか
[ ] サイト全体の他画像と統一感があるか

【法的チェック】
[ ] 商用利用可プランで生成したか
[ ] 実在人物・ブランドと酷似していないか
[ ] 著作権のある作品と酷似していないか
```

---

## 11. AI 画像使用 NG 事例

以下の用途では AI 生成画像を**使わない**：

### 絶対 NG

1. **施工事例・実例写真**: 虚偽表示になる
2. **スタッフ・代表者の写真**: 個人特定のため実写必須
3. **店舗・施設の外観／内観**: 実際と異なる
4. **商品写真**（実物が特定される場合）
5. **症例・治療事例**（医療広告等ガイドライン違反）
6. **契約済み物件の写真**（不動産）
7. **料理の実際のメニュー写真**（景表法違反の可能性）

### 条件付き OK

1. **ヒーロー画像の抽象イメージ**: 「～をイメージした」と明示
2. **業種のイメージカット**: 特定物件・商品と誤認されないもの
3. **背景・パターン**: 純粋なデザイン要素
4. **アイコン・ピクトグラム**: シンプルな図形

---

## 12. 商用利用ライセンスの確認

各 AI 画像生成サービスの最新ライセンスを確認：

### 2026年時点の主要サービス

| サービス | 商用利用 | 注意点 |
|---|---|---|
| Midjourney | 有料プランで可 | 無料トライアルは個人利用のみ |
| DALL-E 3（ChatGPT） | 可 | ユーザーに所有権 |
| Stable Diffusion | 可（モデルによる） | モデルライセンス確認 |
| Adobe Firefly | 可 | 商用安全、Adobe が補償 |
| Leonardo AI | 有料プランで可 | プラン確認 |

**最新情報は各公式サイトで確認**。判例・規制の変化に注意。

---

## 13. AI 画像生成の限界

AI 画像が得意でない領域：

1. **特定の日本語テキストを含む画像**（看板等の日本語がうまく生成されない）
2. **複雑な手・指**（不自然になりやすい）
3. **特定の商品ロゴ**（既存ブランドと酷似リスク）
4. **建物の正確な描写**（実在物件と混同）
5. **文化的に繊細な表現**（宗教・政治等）

これらの領域では、**実写撮影**か**プロのイラストレーター**を使用。

---

## 14. プロンプトのブラッシュアップ例

初回プロンプトから改善していく例：

### 初回プロンプト
```
A Japanese house
```

### 改善1: スタイル追加
```
A modern Japanese house, photography style
```

### 改善2: 詳細追加
```
A modern Japanese house with wooden accents, 
natural lighting, photography style
```

### 改善3: 雰囲気・色調
```
A modern Japanese house with warm wooden accents, 
soft afternoon sunlight, clear blue sky, 
photography style, 
warm and inviting atmosphere
```

### 改善4: 構図・品質
```
A modern Japanese house with warm wooden accents, 
soft afternoon sunlight, clear blue sky, 
photography style, 
warm and inviting atmosphere, 
wide shot from slightly below, 
professional photography, 
8k quality, 16:9 aspect ratio, highly detailed
```

プロンプトは**段階的に具体化**していくのがコツ。
