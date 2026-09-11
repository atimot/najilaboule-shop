---
version: alpha
name: Naji la boule Shop
description: 銀座の和食店「Naji la boule」の伊彌彦米オンラインショップ (Shopify Dawn)。LP と同じ焦茶の闇・ひと筋の金・明朝体だけの世界観を、Dawn の設定値だけで再現する。
omitted:
  - section: spacing
    reason: "余白は Dawn のセクション設定 (padding_top / padding_bottom、spacing_grid_*) に委ね、LP の spacing トークンと CSS 層は追従しない (2026-09-11 決定)"
colors:
  # LP の DESIGN.md (najilaboule/DESIGN.md) と同名のトークンは同じ値を保つ。`design.md diff` で drift を検出する
  primary: "#C8A67B"
  accent: "#C8A67B"
  brand: "#241816"
  brand-dark: "#1f1513"
  brand-light: "#2a1d1b"
  text: "#f8f8f8"
  # ここから Shopify 固有。Dawn は本文を text の 75% で描く。その合成値 (rgba(248,248,248,.75) on brand)
  text-body: "#c3c0bf"
typography:
  # Dawn (assets/base.css) が実際に描く値。fontSize は 750px 以上、モバイル値は本文 Typography の表
  display:
    fontFamily: Zen Old Mincho, serif
    fontSize: "52px"
    fontWeight: 400
    lineHeight: 1.3
    letterSpacing: "0.06rem"
  headline:
    fontFamily: Zen Old Mincho, serif
    fontSize: "40px"
    fontWeight: 400
    lineHeight: 1.3
    letterSpacing: "0.06rem"
  title:
    fontFamily: Zen Old Mincho, serif
    fontSize: "24px"
    fontWeight: 400
    lineHeight: 1.3
    letterSpacing: "0.06rem"
  subtitle:
    fontFamily: Zen Old Mincho, serif
    fontSize: "18px"
    fontWeight: 400
    lineHeight: 1.3
    letterSpacing: "0.06rem"
  body:
    fontFamily: Zen Old Mincho, serif
    fontSize: "16px"
    fontWeight: 400
    lineHeight: 1.8
    letterSpacing: "0.06rem"
  button:
    fontFamily: Zen Old Mincho, serif
    fontSize: "15px"
    fontWeight: 400
    lineHeight: 1.2
    letterSpacing: "0.1rem"
  price:
    fontFamily: Zen Old Mincho, serif
    fontSize: "16px"
    fontWeight: 400
    lineHeight: 1.5
    letterSpacing: "0.1rem"
  price-large:
    fontFamily: Zen Old Mincho, serif
    fontSize: "18px"
    fontWeight: 400
    lineHeight: 1.5
    letterSpacing: "0.13rem"
  label:
    fontFamily: Zen Old Mincho, serif
    fontSize: "10px"
    fontWeight: 400
    lineHeight: 1.2
    letterSpacing: "0.13rem"
  badge:
    fontFamily: Zen Old Mincho, serif
    fontSize: "12px"
    fontWeight: 400
    lineHeight: 1
    letterSpacing: "0.1rem"
  caption:
    fontFamily: Zen Old Mincho, serif
    fontSize: "10px"
    fontWeight: 400
    lineHeight: 1.7
    letterSpacing: "0.07rem"
rounded:
  none: "0px"
  full: "9999px"
components:
  # Dawn の primary ボタン (カートに追加・購入・送信)。EC の例外として静止時から金塗り (LP の One Gold Rule は適用しない)
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.brand}"
    typography: "{typography.button}"
    rounded: "{rounded.none}"
    padding: "0 30px"
    height: "45px"
  # button_style_secondary。地は scheme 背景 (= brand)、白 1px の枠
  button-secondary:
    backgroundColor: "{colors.brand}"
    textColor: "{colors.text}"
    typography: "{typography.button}"
    rounded: "{rounded.none}"
    padding: "0 30px"
    height: "45px"
  hero-heading:
    textColor: "{colors.text}"
    typography: "{typography.display}"
  page-heading:
    textColor: "{colors.text}"
    typography: "{typography.headline}"
  section-heading:
    textColor: "{colors.text}"
    typography: "{typography.title}"
  card-heading:
    textColor: "{colors.text}"
    typography: "{typography.subtitle}"
  body-text:
    textColor: "{colors.text-body}"
    typography: "{typography.body}"
  section-label:
    textColor: "{colors.text-body}"
    typography: "{typography.label}"
  product-price:
    textColor: "{colors.text}"
    typography: "{typography.price-large}"
  card-price:
    textColor: "{colors.text}"
    typography: "{typography.price}"
  footer-text:
    textColor: "{colors.text-body}"
    typography: "{typography.caption}"
  # card_style: standard、card_color_scheme: scheme-3
  card:
    backgroundColor: "{colors.brand}"
    textColor: "{colors.text}"
    rounded: "{rounded.none}"
  # sale_badge_color_scheme: scheme-4 (金地)
  badge-sale:
    backgroundColor: "{colors.accent}"
    textColor: "{colors.brand}"
    typography: "{typography.badge}"
    rounded: "{rounded.none}"
  # sold_out_badge_color_scheme: scheme-3
  badge-sold-out:
    backgroundColor: "{colors.brand}"
    textColor: "{colors.text}"
    typography: "{typography.badge}"
    rounded: "{rounded.none}"
  # 容量バリエーション (picker_type: button)。variant_pills_radius: 0
  variant-pill:
    backgroundColor: "{colors.brand}"
    textColor: "{colors.text}"
    rounded: "{rounded.none}"
  input:
    backgroundColor: "{colors.brand}"
    textColor: "{colors.text}"
    rounded: "{rounded.none}"
  # 色見本 (swatch_shape: circle)。角丸が現れる唯一の要素
  swatch:
    rounded: "{rounded.full}"
  # 予備スキーム。scheme-2 = brand-light、scheme-5 = brand-dark (テーマエディタで選んでもパレット内に収まる)
  surface-light:
    backgroundColor: "{colors.brand-light}"
    textColor: "{colors.text}"
  surface-dark:
    backgroundColor: "{colors.brand-dark}"
    textColor: "{colors.text}"
---

<!--
  このファイルは Google Labs の DESIGN.md オープン仕様 (https://github.com/google-labs-code/design.md) に従う。
  - フロントマターのトークンが正 (normative)。本文はその使いどころと、Shopify (Dawn) の設定値との対応。
  - 上流は LP の DESIGN.md (https://github.com/atimot/najilaboule/blob/main/DESIGN.md、ローカルは /Users/tomitad/work/najilaboule/DESIGN.md)。
    LP と同名のトークン (primary / accent / brand / brand-dark / brand-light / text) は同じ値を保つ。それ以外は Dawn が実際に描く値を書く。
  - 最終同期: LP の DESIGN.md コミット 0fcf8b6 (2026-09-10)。
  - 検証: `npx -y @google/design.md@0.4.0 lint DESIGN.md` (エラー 0 を維持する)
  - drift 検出: `npx -y @google/design.md@0.4.0 diff /Users/tomitad/work/najilaboule/DESIGN.md DESIGN.md` で
    colors の modified が空であることを確認する (added / removed は両プロジェクト固有のトークンなので出てよい)
  - 同期するのは `config/settings_data.json` の設定値のみ。CSS / Liquid の追記で LP に寄せることはしない (CLAUDE.md「デザイン決め事」)
-->

# Design System: Naji la boule Shop

## Overview

**Creative North Star: 「宵闇のカウンター、その先の台所 (The Counter at Nightfall, and the Kitchen Beyond)」**

LP が再現する「灯りを落とした銀座のカウンター」の空気を、そのまま持ち帰れる場所。店で出会った伊彌彦米を、家の食卓に運ぶための EC。面は LP と同じ焦茶一色、差し色は金ひと筋、文字は明朝体だけ。ただし目的は「買う」ことなので、静かさは保ちつつ、商品写真と購入導線だけは明快にする。

このテーマは Shopify Dawn をベースにしており、世界観の再現は **`config/settings_data.json` の設定値だけ** で行う。LP の余白・行間・モーション・背景の質感は CSS でしか再現できないため追従しない。

**Key Characteristics:**
- 全ページが 1 つのダークスキーム (`brand` #241816 の地に `text` #f8f8f8) で統一され、明るい面は存在しない
- 金 (`accent` #C8A67B) はボタンとセールバッジにだけ現れる。EC の例外として、購入系のボタンは静止時から金で塗る
- 書体は Zen Old Mincho 1 書体 (Shopify のフォントライブラリ `zen_old_mincho_n4`)。見出しも本文も価格も同じ書体
- 角丸も影もない。丸いのは色見本だけ
- 商品写真は減光しない。減光と焦茶へのグラデーションは LP の演出写真のためのルールで、商品の姿は正しく見せる
- 余白の広さは Dawn のセクション設定で確保する (数値は Layout 参照)

## Colors

パレットは LP と共通の「焦茶の階調 + 白 + 金 1 色」。Dawn の 5 つのカラースキームは、すべてこのパレットの中に収める。

### Primary
- **金 (`primary` / `accent`, #C8A67B)**: 唯一の差し色。Dawn の primary ボタン (`button`) の地色、セールバッジ (`scheme-4`) の地色に使う。LP では金は「触れている間」だけ面になるが、ショップでは購入導線の明快さを優先し、カートに追加・購入・送信のボタンは静止時から金で塗る。それ以外の面を金で塗らない。

### Neutral
- **焦茶 (`brand`, #241816)**: 全ページの背景。`scheme-1` (body の地色になる) と `scheme-3` (全セクションが使う) の `background`
- **深い焦茶 (`brand-dark`, #1f1513)**: 予備スキーム `scheme-5` の背景。現在どのセクションも使っていない
- **浮いた焦茶 (`brand-light`, #2a1d1b)**: 予備スキーム `scheme-2` の背景。現在どのセクションも使っていない
- **本文の白 (`text`, #f8f8f8)**: 見出し・価格・ボタン文字・入力文字。各スキームの `text`
- **沈めた白 (`text-body`, #c3c0bf)**: Dawn が本文と補足文字を `text` の 75% で描く際の実効色。LP の `text-muted` に相当し、暗背景で許容する最も薄い文字色

### Shopifyへの対応
`config/settings_data.json` の `current.color_schemes` との対応。`presets.Dawn` は Dawn のリセット用なので触らない。

| スキーム | background | text | button | button_label | secondary_button_label | 用途 |
|---|---|---|---|---|---|---|
| scheme-1 | `brand` | `text` | `accent` | `brand` | `text` | body の地色 (Dawn は最初のスキームを `:root` に流し込む)、商品ページ |
| scheme-2 | `brand-light` | `text` | `accent` | `brand` | `text` | 予備 |
| scheme-3 | `brand` | `text` | `accent` | `brand` | `text` | ヘッダー・フッター・全セクション・カード・カート・売切バッジ |
| scheme-4 | `accent` | `brand` | `brand` | `accent` | `brand` | セールバッジのみ |
| scheme-5 | `brand-dark` | `text` | `accent` | `brand` | `text` | 予備 |

`shadow` は全スキームで `#000000` だが、影の不透明度がすべて 0 なので描画されない。

### Named Rules
**The Single-Scheme Rule.** 新しいセクションを足すときは `color_scheme` を `scheme-3` にする。明るいスキームは存在しないので、白い面が現れたら設定ミス。

**The Gold-for-Purchase Rule.** 金で塗ってよい面は、購入系の primary ボタンとセールバッジだけ。それ以外の CTA (ヒーローの「お米を撰ぶ」など) は `button_style_secondary: true` の白罫線にする。

## Typography

**Display / Body Font:** Zen Old Mincho (和文・欧文とも) → serif。`type_header_font` と `type_body_font` の両方を `zen_old_mincho_n4` にする。
**Label Font:** 同上。サンセリフは導入しない。

**Character:** LP と同じ明朝 1 書体。Dawn は太字を `weight + 300` で作るので 700 が読み込まれ、それ以外のウェイトは使われない。字間・行間は Dawn の既定値 (字間 0.06rem、本文行間 1.8) のままで、LP の 0.1em / 2 には寄せない。

### Hierarchy
右列はモバイル (750px 未満) の値。フロントマターはデスクトップ値。`heading_scale` / `body_scale` は 100 のまま。

- **Display** (`display`, 52px / 1.3 / 0.06rem): `heading_size: h0`。トップのヒーロー見出し「結ぶ、米。」だけ。モバイル 40px
- **Headline** (`headline`, 40px): `h1`。ページタイトル、コレクションバナー。モバイル 30px
- **Title** (`title`, 24px): `h2`。「撰り抜きの、米。」などセクション見出し。モバイル 20px
- **Subtitle** (`subtitle`, 18px): `h3`。カード内の商品名。モバイル 17px
- **Body** (`body`, 16px / 1.8 / 0.06rem): 本文。モバイル 15px。色は `text-body`
- **Button** (`button`, 15px / 1.2 / 0.1rem): すべてのボタン
- **Price** (`price`, 16px / 0.1rem) と **Price Large** (`price-large`, 18px / 0.13rem): カードの価格と商品ページの価格。モバイル 16px
- **Label** (`label`, 10px / 1.2 / 0.13rem、大文字): Dawn の `caption-with-letter-spacing`。LP の仏語小ラベルに相当する唯一の場所
- **Badge** (`badge`, 12px / 1 / 0.1rem): セール・売切バッジ
- **Caption** (`caption`, 10px / 1.7 / 0.07rem): フッターの著作権表記、注記

### Named Rules
**The Serif-Only Rule.** LP と同じ。数字も価格もボタンも明朝。
**The Dawn-Scale Rule.** 文字サイズは Dawn の `h0`〜`h3` と `body` の段階から選ぶ。テーマエディタの `heading_size` で選べる範囲を超えない。

## Layout

**Dawn の既定のグリッドに従う。** ブレークポイントは Dawn の 750px / 990px。LP の「768px 1 本」には合わせない。

**ページ幅。** `page_width: 1200` (コンテンツ幅 120rem、両端 5rem のパディング)。

**セクションの余白。** 各セクションの `padding_top` / `padding_bottom` で作る。`spacing_sections` は 0 のまま (Dawn の margin では body の地色が透けるため)。現在の値: トップの注目商品 80 / 80、コレクションの商品グリッド 60 / 60、カート 60 / 60、お問い合わせ 60 / 80、フッター 60 / 80。新しいセクションは 60〜100 の範囲で「迷ったら広い方」。Dawn の上限は 100。

**グリッド間隔。** `spacing_grid_horizontal` / `spacing_grid_vertical` は 8 (Dawn 既定)。

**ヘッダー。** `logo_position: middle-left`、`sticky_header_type: on-scroll-up`、`padding_top` / `padding_bottom` 20。罫線は消す (`show_line_separator: false`)。

**余白トークンは持たない。** フロントマターの `spacing` は `omitted` で宣言している。LP の 96px / 160px はこのテーマでは再現しない。

## Elevation & Depth

**影は使わない。** `buttons_shadow_opacity`、`variant_pills_shadow_opacity`、`inputs_shadow_opacity`、`card_shadow_opacity`、`collection_card_shadow_opacity`、`blog_card_shadow_opacity`、`text_boxes_shadow_opacity`、`media_shadow_opacity`、`popup_shadow_opacity`、`drawer_shadow_opacity` をすべて 0 にする。

**奥行きは作らない。** LP の背景 3 層 (ノイズとラジアルグラデーション) は CSS が必要なので再現しない。すべての面が同じ `brand` で、階層は罫線の透明度だけで示す: ボタン枠 100% (`buttons_border_opacity`)、入力欄とバリエーションピル 55%、カードとテキストボックス 10%、メディア 5%。

### Named Rules
**The Flat-Dark Rule.** LP と同じ。手前にあるものは「地と同じ色で、光だけが違う」。ショップでは光の代わりに罫線の濃さで示す。

## Shapes

**角は直角。** `buttons_radius`、`variant_pills_radius`、`inputs_radius`、`card_corner_radius`、`collection_card_corner_radius`、`blog_card_corner_radius`、`text_boxes_radius`、`media_radius`、`popup_corner_radius`、`badge_corner_radius` をすべて 0 にする。

**丸いのは色見本だけ。** 商品テンプレートの `swatch_shape: circle` は LP の「円形の要素」に相当する唯一の例外。

**線は 1px。** ボタン、入力欄、バリエーションピル、メディアの枠はすべて `*_border_thickness: 1`。カードとテキストボックスは 0。

**写真の比率。** 商品カードは `image_ratio: adapt` (元画像の比率)。ヒーローは `image_height: large`。

## Components

### Buttons
- **Primary (`button-primary`):** Dawn の primary ボタン。金の地に焦茶の文字、高さ 45px、横 30px。カートに追加・購入手続き・送信に使う。ホバーは Dawn 既定 (枠が太くなるだけで色は変えない)
- **Secondary (`button-secondary`):** `button_style_secondary: true`。地は透明 (= `brand`)、`text` 100% の 1px 枠。ヒーローの「お米を撰ぶ」など、購入以外の CTA はこちら
- **Labels:** 日本語のまま (「お米を撰ぶ」「カートに追加」)。LP のような英字大文字にはしない

### Cards
- `card_style: standard`、`card_color_scheme: scheme-3`、枠 0、`card_image_padding: 0`、`card_text_alignment: left`。写真は減光せず、そのまま置く

### Badges
- **Sale (`badge-sale`):** `scheme-4`、金の地に焦茶の文字。金が面になる例外の 1 つ
- **Sold out (`badge-sold-out`):** `scheme-3`、焦茶の地に白の文字と白の枠
- 位置は `badge_position: bottom left`、角は直角

### Variant Pills & Inputs
- 容量などのバリエーションは `picker_type: button` のピル (`variant-pill`)。角は直角、枠は白 55%
- 入力欄 (`input`) も同じ。枠 1px、白 55%

### Header & Footer
- ヘッダー・アナウンスバー・フッターは `scheme-3`。アナウンスバーは「銀座から、お米を。」を用意しているが、現在は `disabled: true`
- フッターはニュースレター枠オフ、決済アイコンとポリシーリンクを表示。SNS リンクは空

### Cart
- `cart_type: notification`、`cart_color_scheme: scheme-3`

### Copy
- LP の語感を継承する: 体言止め + 読点 (「結ぶ、米。」「撰り抜きの、米。」「銀座から、お米を。」)。「撰ぶ」「撰り抜き」の字は意図的
- 約物は LP と同じ: レンジは en dash `–`、アポストロフィは `’`、三点リーダーは `…`、和文の引用は「」

## Do's and Don'ts

### Do:
- **Do** 色・フォント・角丸・影は `config/settings_data.json` の `current` だけで設定し、LP の DESIGN.md と同名のトークンは同じ値を保つ
- **Do** 新しいセクションは `color_scheme: scheme-3`、角丸 0、影 0、余白 60〜100 で始める
- **Do** 購入以外の CTA は `button_style_secondary: true` の白罫線にする
- **Do** 商品写真はそのままの明るさで置く
- **Do** コピーは体言止め + 読点の語感を守り、約物は `–` `’` `…` を使う
- **Do** `DESIGN.md` を変えたら `npx -y @google/design.md@0.4.0 lint DESIGN.md` を通し、LP との `diff` で colors の modified が空であることを確認する

### Don't:
- **Don't** CSS や Liquid を足して LP の余白・行間・字間・モーション・背景に寄せない。設定値で表現できないものは追従しない
- **Don't** `presets.Dawn` ブロックを編集しない
- **Don't** 明るいカラースキームを作らない。白い面は存在しない
- **Don't** 購入系ボタンとセールバッジ以外を金で塗らない
- **Don't** 角丸・影・サンセリフを持ち込まない。`*_radius` と `*_shadow_opacity` は 0 のまま
- **Don't** 商品写真を減光したりグレースケールにしたりしない。LP の Dimmed Photo Rule は演出写真だけのもの
- **Don't** Dawn の SVG アイコン (カート・検索・メニュー・決済) を消そうとしない。LP の「アイコンを持たない」は EC には適用しない
- **Don't** `type_header_font` と `type_body_font` を別々の書体にしない
