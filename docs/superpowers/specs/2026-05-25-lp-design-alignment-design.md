# LP デザイン踏襲（第1+2層 最小スコープ）

最終更新: 2026-05-25

## 1. 背景

姉妹プロジェクトの LP (`/Users/tomitad/work/najilaboule`) の世界観 — 色・タイポ・余白・コピー語感 — を、Shopify テーマ側に踏襲する。LP の構造そのものは継承しない（CLAUDE.md 方針）。

現状、`config/settings_data.json` のカラースキームとフォント設定は既に LP に寄せ済み (`#241816` / `#C8A67B` / `#f8f8f8` / Shippori Mincho)。主要テンプレートも `scheme-3` (ダーク) を採用済み。しかしその上の層（コピー文言・余白・細部設定）が Dawn のデフォルトのまま残っており、世界観が一致しない。

## 2. スコープ

### In scope（今回やる）

LP DESIGN.md の正に対して、**設定値の見直しとテンプレート JSON の文言差し替え** のみを行う:

- `config/settings_data.json` の数値・色スキーム選択
- `templates/*.json` および `sections/*-group.json` の `settings.*` の値
- 既存セクションのテキストブロック内コピー

### Out of scope（今回やらない）

- Liquid セクション / スニペットの編集（`sections/*.liquid`, `snippets/*.liquid`）
- CSS ファイルの追記・変更（`assets/*.css`）
- 新規セクションの追加
- トップページの構成変更（`image_banner` + `featured_collection` の2セクション構成は維持）
- LP の `BrandDots` 等、固有装飾の再現
- 背景3層（ノイズ + radial gradient）の再現
- フォーカスリング色のアクセント色への変更

## 3. 設計

### 3.1 余白方針

LP の「大胆な余白」を Shopify の設定枠で表現。値はマーチャントが後でテーマエディタから微調整可能。

#### 共通設定 (`config/settings_data.json` の `current` 直下)

| キー | 現状 | 変更後 | 意図 |
|---|---|---|---|
| `spacing_sections` | 0 | **24** | セクション間にゆとり |
| `cart_color_scheme` | `scheme-1` | **`scheme-3`** | カート通知をダーク統一 |
| `card_style` | `card` | **`standard`** | ダーク背景でカード枠が浮くのを避ける |

#### `templates/index.json`

| セクション | キー | 現状 | 変更後 |
|---|---|---|---|
| `featured_collection` | `padding_top` | 44 | **80** |
| `featured_collection` | `padding_bottom` | 36 | **80** |

注: `image_banner` セクションには `padding_top/bottom` 設定が存在しないため、画像のみで余白感を出す。`image_height: "large"` と `image_overlay_opacity: 40` は維持。

#### `templates/collection.json`

| セクション | キー | 現状 | 変更後 |
|---|---|---|---|
| `banner` | `padding_top` | (未設定/0) | **80** |
| `banner` | `padding_bottom` | (未設定/0) | **40** |
| `product-grid` | `padding_top` | 36 | **60** |
| `product-grid` | `padding_bottom` | 36 | **60** |

注: `main-collection-banner` の schema に `padding_top/bottom` が存在することは前提とするが、実装フェーズで存在確認すること。存在しない場合は当該変更をスキップする。

#### `templates/cart.json`

| セクション | キー | 現状 | 変更後 |
|---|---|---|---|
| `cart-items` | `padding_top` | 36 | **60** |
| `cart-items` | `padding_bottom` | 36 | **60** |
| `cart-footer` | `padding_top` | 20 | **32** |
| `cart-footer` | `padding_bottom` | 40 | **60** |

#### `templates/page.contact.json`

| セクション | キー | 現状 | 変更後 |
|---|---|---|---|
| `form` | `padding_top` | 36 | **60** |
| `form` | `padding_bottom` | 36 | **80** |

注: `main` セクション (`main-page`) の `padding_top/bottom` は 36/36 のまま維持（コンテンツ密度の低いページのため）。

#### `sections/footer-group.json`

| セクション | キー | 現状 | 変更後 |
|---|---|---|---|
| `footer` | `padding_top` | 36 | **60** |
| `footer` | `padding_bottom` | 36 | **80** |

#### `sections/header-group.json`

ヘッダー本体の `padding_top/bottom: 20` は維持（固定ヘッダーで嵩を増やしたくない）。後述の `show_line_separator` のみ変更。

---

### 3.2 コピー方針

LP の語感（体言止め + 「、」/ 仏語小ラベル / 静的な動詞）を踏襲。今回は **最も LP の「体言止め + 「、」」 を強く出すセット** を採用。

| 場所 | ファイル | キー | 現状 | 変更後 |
|---|---|---|---|---|
| アナウンスバー | `sections/header-group.json` | `sections.announcement-bar.blocks.announcement-bar-0.settings.text` | `Welcome to our store` | **`銀座から、お米を。`** |
| ヒーロー大見出し | `templates/index.json` | `sections.image_banner.blocks.heading.settings.heading` | `Browse our latest products` | **`結ぶ、米。`** |
| ヒーロー CTA | `templates/index.json` | `sections.image_banner.blocks.button.settings.button_label_1` | `Shop all` | **`お米を撰ぶ`** |
| 注目商品見出し | `templates/index.json` | `sections.featured_collection.settings.title` | `Featured products` | **`撰り抜きの、米。`** |
| ニュースレター見出し | `sections/footer-group.json` | `sections.footer.settings.newsletter_heading` | `Subscribe to our emails` | **`おたよりを、受け取る。`** |
| コンタクト見出し | `templates/page.contact.json` | `sections.form.settings.heading` | (空) | **`お問い合わせ`** |

注:
- 「撰ぶ」「撰り抜き」は通常の「選ぶ」と異なる漢字を意図的に採用。LP の静謐感を強める意図。
- アナウンスバーは LP の体言止め語感を守りつつ、店としての打ち出しコピーになっている。

---

### 3.3 その他細部

| 場所 | ファイル | キー | 現状 | 変更後 | 意図 |
|---|---|---|---|---|---|
| アナウンスバー | `sections/header-group.json` | `sections.announcement-bar.settings.show_line_separator` | true | **false** | LP は罫線で区切らない |
| ヘッダー | `sections/header-group.json` | `sections.header.settings.show_line_separator` | true | **false** | 同上 |

維持する設定（変更しない）:

- `header.enable_country_selector` / `enable_language_selector`: true → 維持（LP も ja/en あり）
- `header.enable_customer_avatar`: true → 維持
- `cart_type: "notification"` → 維持
- `predictive_search_show_price` / `predictive_search_show_vendor`: false → 維持
- `image_banner.color_scheme: scheme-3` → 維持
- `featured_collection.image_ratio: adapt` → 維持

---

## 4. 変更ファイル一覧

実装時に編集するファイル:

1. `config/settings_data.json`
2. `templates/index.json`
3. `templates/collection.json`
4. `templates/cart.json`
5. `templates/page.contact.json`
6. `sections/header-group.json`
7. `sections/footer-group.json`

`presets.Dawn` ブロックは Dawn テーマのリセット用なので **変更しない**（`current` ブロックのみ更新）。

## 5. 検証

実装後、以下を確認する:

1. `shopify theme dev --store najilaboule.myshopify.com` でローカル起動
2. http://127.0.0.1:9292 で以下を視覚確認:
   - トップページ: アナウンスバー文言、ヒーロー見出し・CTA、注目商品見出し、セクション間余白
   - コレクションページ: バナー余白、商品グリッド余白、カード枠が消えていること
   - カートページ: 余白、カラースキーム
   - コンタクトページ: 見出し、余白
   - フッター: 罫線が消えていること、ニュースレター見出し、余白
3. テーマエディタ (admin) で「カスタマイズ」を開き、各設定がエディタ上でも編集可能であることを確認

## 6. リスクと対策

| リスク | 対策 |
|---|---|
| Dawn の section schema に `padding_top/bottom` が存在しないセクション設定 | 実装時に各 schema を grep で確認し、無いキーは設定追加しない |
| マーチャントが後でテーマエディタから上書きすると `current` ブロックが書き換わる | settings_data.json の冒頭コメントどおり、自動生成ファイルである旨を踏まえ、今回の変更も「初期値の見直し」として扱う |
| ヘッダー罫線を消すと識別性が落ちる懸念 | scheme-3 のダーク背景と本体のコントラストで十分視認可能。気になればテーマエディタで戻せる |

## 7. 追補（実装中の発見と方針修正）

実装フェーズで「あちらこちらに白い余白が見える」という事象を発見。原因と対処を整理し、本仕様にフィードバックした記録。

### 7.1 発見した原因

`layout/theme.liquid` 57-95 行で、Dawn は **`color_schemes` の最初の scheme（= scheme-1）の `background` を `:root` のデフォルト `--color-background` に流し込む** 設計。これが `body` の地色になる。

結果として:

1. **scheme-1 の `background: #fbf7f1`（オフホワイト）が body 地色** となり、ダークなセクション同士の間 (`spacing_sections: 24`) に白帯が走った
2. `card_color_scheme: scheme-2`（オフベージュ）が商品カード地色になり、商品配置時に白浮きする予兆

### 7.2 方針修正

| 当初仕様 (§3.1) | 修正後 | 理由 |
|---|---|---|
| `spacing_sections`: 0 → **24** | **0 維持** | LP の方針通り「各セクションの padding で余白を作る」に揃える。Dawn の margin-top では scheme-1 が透ける |
| `scheme-1` は維持 | **ダーク化**（background `#241816` / text `#f8f8f8` / button `#c8a67b` / button_label `#241816` / secondary_button_label `#f8f8f8` / shadow `#000000`） | body 地色を根本的にダーク化。LP の brand 色と一致 |
| `card_color_scheme`: 維持 (scheme-2) | **scheme-3** | 商品カード背景もダーク化 |
| `collection_card_color_scheme`: 維持 (scheme-2) | **scheme-3** | コレクションカード同上 |
| `blog_card_color_scheme`: 維持 (scheme-2) | **scheme-3** | ブログカード同上 |
| `form.heading`: 空 → **`お問い合わせ`** (§3.2) | **空 維持** | `main-page` セクションのページタイトルと重複したため。当初決定を撤回 |

### 7.3 検証結果（2026-05-25 実施）

`shopify theme dev --store najilaboule.myshopify.com` を起動し Playwright で各ページの全画面スクリーンショットを取得・DOM 解析:

- トップ・コレクション・カート・コンタクト・フッター すべて完全ダーク基調 (#241816) で統一
- 白い余白は全画面で消失
- アクセント色 #c8a67b の CTA（「送信する」「買い物を続ける」）が映える

---

## 8. Future work（今回スコープ外、将来検討）

第3層（CSS 追加）以降が必要になる項目を記録:

- `body` 背景3層の再現（SVG ノイズ + radial gradient × 2）
- フォーカスリングをアクセント色 #C8A67B / offset 2px に
- 仏語小ラベル用ユーティリティクラス（`text-xs / letter-spacing: 0.3em / color: accent`）
- 電話番号・営業時間の `tabular-nums`
- LP の `BrandDots` 装飾要素の再現（第4層、慎重に判断）
