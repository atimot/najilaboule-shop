# LP デザイン踏襲 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Shopify テーマの設定値・テンプレート JSON を LP の世界観（色・余白・コピー語感）に寄せる。CSS / Liquid / 新規セクションには触れず、JSON 値の編集のみで完結する。

**Architecture:** spec (`docs/superpowers/specs/2026-05-25-lp-design-alignment-design.md`) で確定した「第1+2層 最小スコープ」に従う。テスト枠組みが無いプロジェクトのため、各タスクは「該当箇所を Read で確認 → Edit で値を差し替え」の単純構造。全タスク完了後に `shopify theme dev` でローカル目視確認 (Task 8)。

**Tech Stack:** Shopify Dawn テーマ / 編集対象は JSON ファイルのみ / 検証は `shopify theme dev --store najilaboule.myshopify.com` (CLAUDE.md 既定)

**コミット方針:** 各タスクの末尾に「コミット可否をユーザーに確認する」ステップを置く。spec フェーズで「コミットは保留」とされたため、実装フェーズでも明示確認を取る。

**事前確認済み:**
- `main-collection-banner` の schema に `padding_top/bottom` は存在しない → banner padding は変更しない
- `image-banner` の schema に `padding_top/bottom` は存在しない → 変更しない
- `featured-collection` / `product-grid` / `cart-items` / `cart-footer` / `contact-form` / `main-page` / `footer` は既に JSON で padding 値が書かれているため schema 存在は確実

---

### Task 1: 共通設定値の変更

**Files:**
- Modify: `config/settings_data.json`

LP のダーク世界観統一とセクション間余白の確保。`current` ブロックの3つの値を変更。`presets.Dawn` ブロックは触らない。

- [ ] **Step 1: 該当箇所を確認**

Read `config/settings_data.json` の 18 行目付近 (`spacing_sections`)、44 行目付近 (`card_style`)、128 行目付近 (`cart_color_scheme`) を確認。

- [ ] **Step 2: `spacing_sections` を 0 → 24 に変更**

Edit:

```
    "page_width": 1200,
    "spacing_sections": 0,
```

→

```
    "page_width": 1200,
    "spacing_sections": 24,
```

- [ ] **Step 3: `card_style` を card → standard に変更**

Edit:

```
    "card_style": "card",
```

→

```
    "card_style": "standard",
```

- [ ] **Step 4: `cart_color_scheme` を scheme-1 → scheme-3 に変更**

Edit:

```
    "cart_color_scheme": "scheme-1",
```

→

```
    "cart_color_scheme": "scheme-3",
```

- [ ] **Step 5: ユーザーにコミット可否を確認**

「Task 1 (共通設定 3 件) を完了。コミットしますか？」と尋ね、指示に従う。コミットする場合のメッセージ例:

```
chore(theme): set spacing_sections, card_style, cart_color_scheme for LP alignment
```

---

### Task 2: ヘッダーグループの変更

**Files:**
- Modify: `sections/header-group.json`

アナウンスバーの文言を LP 語感に、罫線をオフに（LP は罫線を使わない）。

- [ ] **Step 1: 該当箇所を確認**

Read `sections/header-group.json` を全文確認。20 行目付近の `text`、33 行目付近の `show_line_separator` (announcement-bar)、46 行目付近の `show_line_separator` (header) を把握。

- [ ] **Step 2: アナウンスバーの text を変更**

Edit:

```
            "text": "Welcome to our store",
```

→

```
            "text": "銀座から、お米を。",
```

- [ ] **Step 3: announcement-bar の `show_line_separator` を true → false**

該当ブロック（announcement-bar セクション内）の以下を変更。`replace_all` は使わず、announcement-bar 側のみ Edit:

```
        "color_scheme": "scheme-3",
        "show_line_separator": true,
        "show_social": false,
```

→

```
        "color_scheme": "scheme-3",
        "show_line_separator": false,
        "show_social": false,
```

- [ ] **Step 4: header セクションの `show_line_separator` を true → false**

header セクション側の該当行を Edit:

```
        "sticky_header_type": "on-scroll-up",
        "show_line_separator": true,
        "color_scheme": "scheme-3",
```

→

```
        "sticky_header_type": "on-scroll-up",
        "show_line_separator": false,
        "color_scheme": "scheme-3",
```

- [ ] **Step 5: ユーザーにコミット可否を確認**

コミット時のメッセージ例:

```
feat(theme): update announcement copy and remove header dividers
```

---

### Task 3: フッターグループの変更

**Files:**
- Modify: `sections/footer-group.json`

フッター余白の拡張とニュースレター見出しの語感調整。

- [ ] **Step 1: 該当箇所を確認**

Read `sections/footer-group.json` で 19 行目付近の `newsletter_heading`、27〜28 行目付近の `padding_top`/`padding_bottom` を確認。

- [ ] **Step 2: `newsletter_heading` を変更**

Edit:

```
        "newsletter_heading": "Subscribe to our emails",
```

→

```
        "newsletter_heading": "おたよりを、受け取る。",
```

- [ ] **Step 3: footer の `padding_top` / `padding_bottom` を 36/36 → 60/80 に変更**

Edit:

```
        "margin_top": 0,
        "padding_top": 36,
        "padding_bottom": 36
```

→

```
        "margin_top": 0,
        "padding_top": 60,
        "padding_bottom": 80
```

- [ ] **Step 4: ユーザーにコミット可否を確認**

コミット時のメッセージ例:

```
feat(theme): adjust footer copy and spacing for LP alignment
```

---

### Task 4: トップページの変更

**Files:**
- Modify: `templates/index.json`

ヒーロー文言・CTA・注目商品見出し・余白を LP 語感に。

- [ ] **Step 1: 該当箇所を確認**

Read `templates/index.json` で以下を把握:
- 18 行目付近: ヒーロー heading
- 25〜26 行目付近: ヒーロー CTA `button_label_1` と `button_link_1`
- 57 行目付近: featured_collection の `title`
- 76〜77 行目付近: featured_collection の `padding_top`/`padding_bottom`

- [ ] **Step 2: ヒーロー heading を変更**

Edit:

```
            "heading": "Browse our latest products",
            "heading_size": "h0"
```

→

```
            "heading": "結ぶ、米。",
            "heading_size": "h0"
```

- [ ] **Step 3: ヒーロー CTA `button_label_1` を変更**

Edit:

```
            "button_label_1": "Shop all",
            "button_link_1": "shopify://collections/all",
```

→

```
            "button_label_1": "お米を撰ぶ",
            "button_link_1": "shopify://collections/all",
```

- [ ] **Step 4: featured_collection の `title` を変更**

Edit:

```
        "title": "Featured products",
```

→

```
        "title": "撰り抜きの、米。",
```

- [ ] **Step 5: featured_collection の `padding_top` / `padding_bottom` を 44/36 → 80/80 に変更**

Edit:

```
        "padding_top": 44,
        "padding_bottom": 36
```

→

```
        "padding_top": 80,
        "padding_bottom": 80
```

- [ ] **Step 6: ユーザーにコミット可否を確認**

コミット時のメッセージ例:

```
feat(theme): update homepage copy and spacing for LP alignment
```

---

### Task 5: コレクションテンプレートの変更

**Files:**
- Modify: `templates/collection.json`

product-grid の余白のみ変更（banner は schema に padding 設定が存在しないため変更不可、spec 通りスキップ）。

- [ ] **Step 1: 該当箇所を確認**

Read `templates/collection.json` で 36〜37 行目付近の `product-grid` の `padding_top`/`padding_bottom` を確認。

- [ ] **Step 2: product-grid の `padding_top` / `padding_bottom` を 36/36 → 60/60 に変更**

Edit:

```
        "padding_top": 36,
        "padding_bottom": 36
```

→

```
        "padding_top": 60,
        "padding_bottom": 60
```

- [ ] **Step 3: ユーザーにコミット可否を確認**

コミット時のメッセージ例:

```
feat(theme): expand collection grid padding for LP alignment
```

---

### Task 6: カートテンプレートの変更

**Files:**
- Modify: `templates/cart.json`

cart-items と cart-footer の余白拡張。

- [ ] **Step 1: 該当箇所を確認**

Read `templates/cart.json` で 16〜17 行目付近の `cart-items` padding と 38〜39 行目付近の `cart-footer` padding を確認。

- [ ] **Step 2: cart-items の padding を 36/36 → 60/60 に変更**

Edit:

```
      "settings": {
        "color_scheme": "scheme-3",
        "padding_top": 36,
        "padding_bottom": 36
      }
```

→

```
      "settings": {
        "color_scheme": "scheme-3",
        "padding_top": 60,
        "padding_bottom": 60
      }
```

(同じ "padding_top": 36 が cart-footer にも 20 / cart-items に 36 と異なるので、cart-items 側のブロック全体で一意であることを確認した上で Edit 適用)

- [ ] **Step 3: cart-footer の padding を 20/40 → 32/60 に変更**

Edit:

```
        "color_scheme": "scheme-3",
        "padding_top": 20,
        "padding_bottom": 40
```

→

```
        "color_scheme": "scheme-3",
        "padding_top": 32,
        "padding_bottom": 60
```

- [ ] **Step 4: ユーザーにコミット可否を確認**

コミット時のメッセージ例:

```
feat(theme): expand cart paddings for LP alignment
```

---

### Task 7: コンタクトページの変更

**Files:**
- Modify: `templates/page.contact.json`

フォーム見出しの追加と form セクションの余白拡張。`main` セクションの padding は変更しない（spec 通り維持）。

- [ ] **Step 1: 該当箇所を確認**

Read `templates/page.contact.json` で 22 行目付近の `heading` (空)、25〜26 行目付近の form 側 padding を確認。

- [ ] **Step 2: form の `heading` を空 → `お問い合わせ` に変更**

Edit:

```
      "settings": {
        "heading": "",
        "heading_size": "h1",
```

→

```
      "settings": {
        "heading": "お問い合わせ",
        "heading_size": "h1",
```

- [ ] **Step 3: form の `padding_top` / `padding_bottom` を 36/36 → 60/80 に変更**

Edit:

```
        "color_scheme": "scheme-3",
        "padding_top": 36,
        "padding_bottom": 36
```

→

```
        "color_scheme": "scheme-3",
        "padding_top": 60,
        "padding_bottom": 80
```

- [ ] **Step 4: ユーザーにコミット可否を確認**

コミット時のメッセージ例:

```
feat(theme): add contact heading and expand form spacing for LP alignment
```

---

### Task 8: 目視検証

**Files:** なし（編集なし、確認のみ）

ローカル開発サーバで全体を視覚確認。

- [ ] **Step 1: 開発サーバ起動の準備**

CLAUDE.md に従い、コマンドはユーザー側で実行する想定:

```
shopify theme dev --store najilaboule.myshopify.com
```

ユーザーに「上記コマンドを別ターミナルで起動してください。終わったらお知らせください」と依頼する（自動起動は dev サーバが長時間プロセスのため、 `run_in_background` するか確認）。

- [ ] **Step 2: 検証チェックリスト**

http://127.0.0.1:9292 でブラウザ目視。spec 5 章のリストを順に確認:

- トップページ
  - [ ] アナウンスバーが `銀座から、お米を。` で、罫線が無い
  - [ ] ヒーロー大見出しが `結ぶ、米。`
  - [ ] ヒーロー CTA が `お米を撰ぶ`
  - [ ] 注目商品見出しが `撰り抜きの、米。`
  - [ ] セクション間に明確な余白
- コレクションページ (`/collections/all` または任意のコレクション)
  - [ ] product-grid の上下余白が広がっている
  - [ ] 商品カードに枠が無い (`card_style: standard` の効果)
- カートページ (`/cart`)
  - [ ] ダーク基調 (scheme-3) で表示
  - [ ] cart-items / cart-footer の余白が広がっている
- コンタクトページ (`/pages/contact`)
  - [ ] 見出し `お問い合わせ` が表示
  - [ ] form 周りに余白
- フッター
  - [ ] 罫線が無い
  - [ ] ニュースレター見出しが `おたよりを、受け取る。`
  - [ ] 下部に余白

- [ ] **Step 3: テーマエディタでの編集可能性確認**

Shopify Admin → オンラインストア → テーマ → カスタマイズ で開発テーマを開き、上記で変更した設定がエディタ上で見え、編集可能であることを軽く確認する。

- [ ] **Step 4: 問題があれば該当 Task に戻って修正、無ければ完了報告**

問題が見つかれば該当の Task 1-7 のいずれかに戻り、その Task のステップだけを再実行する。spec とのズレを発見した場合は、その旨をユーザーに報告し、spec の修正可否を相談する。

---

---

### Task 9: 白余白問題の修正（実装中に追加）

**Files:**
- Modify: `config/settings_data.json`
- Modify: `templates/page.contact.json`

実装後の目視検証で「白い帯がセクション間に走る」事象を発見。原因は Dawn が **`color_schemes` 最初の scheme-1 を `:root` の `--color-background` に流し込む** 仕様で、scheme-1 の `#fbf7f1` が body 地色になり `spacing_sections` の隙間に透けていたこと。spec §7（追補）に方針修正を反映済み。

- [ ] **Step 1: `spacing_sections` を 24 → 0 に戻す**

Edit `config/settings_data.json`:

```
    "page_width": 1200,
    "spacing_sections": 24,
```

→

```
    "page_width": 1200,
    "spacing_sections": 0,
```

- [ ] **Step 2: `scheme-1` をダーク化（current 側のみ）**

Edit `config/settings_data.json`:

```
      "scheme-1": {
        "settings": {
          "background": "#fbf7f1",
          "background_gradient": "",
          "text": "#241816",
          "button": "#241816",
          "button_label": "#fbf7f1",
          "secondary_button_label": "#241816",
          "shadow": "#241816"
        }
      },
```

→

```
      "scheme-1": {
        "settings": {
          "background": "#241816",
          "background_gradient": "",
          "text": "#f8f8f8",
          "button": "#c8a67b",
          "button_label": "#241816",
          "secondary_button_label": "#f8f8f8",
          "shadow": "#000000"
        }
      },
```

- [ ] **Step 3: card / collection_card / blog_card_color_scheme を scheme-3 に変更**

3つの Edit を順に。前後にユニークなコンテキストを取って current 側のみ書き換える:

```
    "card_text_alignment": "left",
    "card_color_scheme": "scheme-2",
```
→
```
    "card_text_alignment": "left",
    "card_color_scheme": "scheme-3",
```

同パターンで `collection_card_color_scheme`、`blog_card_color_scheme`。

- [ ] **Step 4: コンタクトフォーム heading を空に戻す**

`templates/page.contact.json` の form heading は `main-page` のページタイトルと重複したため空に戻す:

```
      "settings": {
        "heading": "お問い合わせ",
        "heading_size": "h1",
```

→

```
      "settings": {
        "heading": "",
        "heading_size": "h1",
```

- [ ] **Step 5: Playwright で再検証**

主要ページ (`/`, `/collections/all`, `/cart`, `/pages/contact`) を navigate して fullPage screenshot を取得し、白余白が消えていることを確認。

- [ ] **Step 6: ユーザーにコミット可否を確認**

コミット時のメッセージ例:

```
fix(theme): make scheme-1 dark and remove section margin gaps
```

---

## Self-Review

**1. Spec coverage:**

| Spec 項目 | Task | カバー状況 |
|---|---|---|
| 3.1 共通設定: `spacing_sections`, `cart_color_scheme`, `card_style` | Task 1 | ✓ |
| 3.1 `templates/index.json` (featured_collection) | Task 4 Step 5 | ✓ |
| 3.1 `templates/collection.json` (banner padding) | – | スキップ（schema 無し、事前確認済み） |
| 3.1 `templates/collection.json` (product-grid padding) | Task 5 | ✓ |
| 3.1 `templates/cart.json` (cart-items, cart-footer padding) | Task 6 | ✓ |
| 3.1 `templates/page.contact.json` (form padding) | Task 7 Step 3 | ✓ |
| 3.1 `sections/footer-group.json` (footer padding) | Task 3 Step 3 | ✓ |
| 3.1 ヘッダー padding は維持 | – | 変更なし（spec 通り） |
| 3.2 アナウンスバー文言 | Task 2 Step 2 | ✓ |
| 3.2 ヒーロー大見出し | Task 4 Step 2 | ✓ |
| 3.2 ヒーロー CTA | Task 4 Step 3 | ✓ |
| 3.2 注目商品見出し | Task 4 Step 4 | ✓ |
| 3.2 ニュースレター見出し | Task 3 Step 2 | ✓ |
| 3.2 コンタクト見出し | Task 7 Step 2 | ✓ |
| 3.3 announcement-bar の `show_line_separator: false` | Task 2 Step 3 | ✓ |
| 3.3 header の `show_line_separator: false` | Task 2 Step 4 | ✓ |
| 5. 検証手順 | Task 8 | ✓ |

**2. Placeholder scan:** TBD / TODO / "implement later" 等なし。各ステップに exact 値が記載されている。

**3. Type/value consistency:** 全ての値が spec の表と一致。
