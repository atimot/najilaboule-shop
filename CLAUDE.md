# Najilaboule Shop (Shopify Theme)

自社の和食店ブランド「Naji la boule」のお米を販売する EC サイト用 Shopify テーマ。
ベースは Shopify 公式テーマ **Horizon 4.1.5** (Theme Store 版)。LP (https://atimot.github.io/najilaboule/) とは **色もフォントも共有しない** (2026-09-24 決定。ブラウンの暗い配色は EC には重いため、ショップ独自の「白 × 墨 × 稲穂の金」に切り替えた)。LP の構造 (店舗紹介・予約導線) も継承しない。

- ストア (管理画面ハンドル): `najilaboule` (admin.shopify.com/store/najilaboule)
- 永続ドメイン: `vuvwb5-6g.myshopify.com` ← CLI 認証・Admin API はこちら (`najilaboule.myshopify.com` は別名。OAuth コールバック不一致でエラーになる)
- 公開: **2026-09-12 に Horizon を公開済み** (当初予定は 2026-10-01。ストアは引き続きパスワード保護中)

## テーマとブランチの対応 (2026-09-12 公開後)

| テーマ | ID | 状態 | 対応ブランチ |
|---|---|---|---|
| Horizon | 145592877107 | **公開中 (MAIN)** | `main` |
| najilaboule-shop/main | 143376744499 | 未公開 (旧 Dawn ベース)。GitHub 連携は 2026-09-12 に解除済み。削除候補 | なし |
| Dawn | 143036547123 | 未公開。旧。削除候補 | なし |

公開中テーマ (Horizon) の内容 = `main` ブランチ。GitHub 連携は使っておらず、反映は Shopify CLI の `push` / `pull` で行う。テーマエディタで変更したら `shopify theme pull` で `main` に取り込んでコミットする。

## デザイン決め事

デザインの正 (source of truth) はこの CLAUDE.md の「デザイン決め事」。LP の `DESIGN.md` は参照しない。

**触ってよいのは設定データだけ** (どれもテーマエディタが書き込むのと同じ JSON):

1. テーマ全体の設定 = `config/settings_data.json` の `current` (テーマエディタの「テーマ設定」)
2. セクション・ブロック単位の設定 = `templates/*.json` と `sections/*-group.json` (テーマエディタでセクションを選んだときの設定。2026-09-24 に追加)

設定できる項目・値・範囲は `config/settings_schema.json` と各セクション / ブロックの `{% schema %}` に定義されている。日本語の表示名は `locales/ja.schema.json`。

**カスタム CSS は使わない** (2026-09-24 決定)。テーマ全体 (`platform_customizations.custom_css`) もセクション単位 (テンプレート JSON 内の `custom_css`) も同じ。理由: テーマ設定の値を上書きして管理画面の設定が効かなくなる。同日に游明朝のため書体だけ例外にしたが、ライブラリの Noto Serif Japanese に切り替えて例外も無くした。テーマ設定で表現できない見た目は、テーマ設定の範囲で近いものに寄せる。

`sections/` `blocks/` `snippets/` `assets/` `layout/` の Liquid / CSS / JS 本体は **編集しない** (Theme Store の更新に追従できなくなるため)。

**カラーパレット (白 × 墨 × 稲穂の金。2026-09-24〜):**

| パレットキー | 値 | 役割 |
|---|---|---|
| background | #ffffff | ページ・ヘッダー・入力欄・カートの地 |
| foreground | #1f1f1f | 墨。文字、主ボタンの地、選択中バリエーションの地 |
| accent | #b8975a | 稲穂の金。セールバッジの地のみ (文字には使わない: 白地に 2.8:1 で読めない) |
| muted | #6b6b6b | 薄い文字 (価格など)、売り切れバッジの文字 |
| surface | #f5f2ec | ごく薄い生成り。副ボタンの地 |
| color1 | #1f1f1f | フッターの地 (文字色はテーマが自動で白にする) |
| color2 | #d9d4cb | 罫線・枠線 |

- ヒーロー (トップ) の見出しとボタンは写真の上なのでパレット参照ではなく白 `#ffffff` を直接指定している
- ヒーロー画像 (2026-09-24): 羽釜の中の炊きたてのご飯としゃもじ (`najila_top6.jpg`。元は `~/work/najilaboule-tmp/03_kama/najila_top6.JPG`)。元の 6000×3376 は Shopify の画像上限 (20MP) を超えるので 3840×2160 に縮小して上げた。ストアの「ファイル」で焦点を横中央・上端 (50% / 0%) に設定済み (PC の横長の切り抜きでご飯を下に寄せ、文言を暗い釜の内側に乗せるため。焦点は API では設定できず、管理画面のファイル編集で行う)。暗幕は単色 #121212 の 55% (`#1212128c`)。40% だと白い文字がご飯に重なって読めない

- フォント: **Noto Serif Japanese** (Shopify フォントライブラリ収録の Web フォント。どの端末でも同じ書体)。本文 / 小見出し / 見出し / アクセントの 4 か所すべて `noto_serif_japanese_n5` (500)。**ウェイトは 1 つだけ** (2026-09-24 決定): Shopify 配信の Noto Serif Japanese は 1 ウェイト約 2.7MB あり、見出し用に 600 を足すと初回 5.4MB になるため。見出しは大きさだけで区別する。h1〜h6 はすべて「見出し」の書体プリセットを使う (h5・h6 の既定は小見出しだが、太さを変えるときに h1〜h6 が一緒に動くよう変更済み)
- 字間: 見出し h1〜h6 は「広め」(0.03em)。本文の字間は設定が無いので標準のまま
- 行間: 本文は「広め」(1.6)
- 角丸はすべて 0、影なし、カードのホバー効果なし
- ボタン: primary = 墨で塗る (地 foreground・文字 background・枠 foreground)、secondary = surface 地・foreground 文字・color2 枠。ホバー色は Horizon の自動計算に任せる
- ロゴ (2026-09-24): **「Naji la boule / ナジラブール」の 2 行。アイコンは入れない** (`najilaboule-wordmark.png`、836×185、墨 #1f1f1f・背景透明)。高さは **PC 32px / スマホ 24px** (控えめにする。ナジラブールは PC で約 7px とほぼ模様扱い)。9 色アイコンはファビコンだけで使う。反転ロゴは未設定 (透明ヘッダーを使っていないので不要)。同日いったんアイコン付き (`najilaboule-logo.png`、PC 48px) にしたが取り下げ
- ファビコン (2026-09-24): 9 色の丸アイコン (`najilaboule-favicon-512.png`、512×512、地色 #231816 のまま。透明にすると白地のタブで白・黄、黒地のタブで黒・紫の丸が消える)。Horizon は 32×32 でしか出力しない
- ロゴ・ファビコンの元画像と生成スクリプトは `~/work/najilaboule-tmp/01_logo/shopify/`。ストアの「ファイル」にあり、設定値は `shopify://shop_images/<ファイル名>`。ファイル名が `icon` などで終わると Shopify が名前に識別子を足すので避ける

**商品詳細ページ (2026-09-24 デザイン診断の反映。`templates/product.json`):**

- 診断の全文は Obsidian `Projects/najilaboule-shop 商品詳細ページのデザイン診断 (2026-09-24).md`。要点: Horizon プリセットのままで「階層・面・写真・文言」が未設計だった
- 階層: 商品名は `h4` プリセット (24px)、価格は `h5` (18px) + `show_tax_info: true` (テーマが「税込。」を出す)。価格の下に 14px・muted の 1 行「送料込み。沖縄・離島・一部地域は +1,000 円を頂戴します。」(`type_preset: custom`、`font_size: 0.875rem`)
- 商品説明 (ストアの商品データ) は **見出しタグを使わない**。`<p>` のリード文 + `<ul>` の内容 + `<p>` の申込期間・発送時期だけ。以前の `<h3>内容</h3>` は h3 プリセット = 商品名と同じ 32px で描画され階層が崩れていたため撤去 (2026-09-24 に 3 商品とも `productUpdate` で書き換え済み)
- 送料・支払いの共通事項は詳細列の末尾の `accordion` ブロック (行「お届けと送料」「お支払い」、`type_preset: h6`、罫線 color2) に置く。商品ごとに違う情報 (内容・期間・発送時期) は説明文に書く
- 詳細列の並び: 商品名 → 価格 → 送料の 1 行 → 区切り線 → 説明 → バリエーション → 購入ボタン → アコーディオン
- 面: 詳細列 (`_product-details`) の背景を `surface`、内側余白は上下 40 / 左右 32 (Horizon がスマホでは左右を 0 にする)。ヘッダーは `border_width: 1` + `bottom_border_color: color2` (メニューが上段のときの下罫線はこの 2 つ。`divider_*` は下段メニュー用)。「ほかのプラン」(旧 You may also like) は背景 `surface`、上下 64、2 列、`max_products: 3` (スキーマの最小値が 3。商品が 2 点なので 2 枚出る)
- ギャラリー: 2 列グリッド、`aspect_ratio: "1"` + `media_fit: cover` で全部を正方形に切り抜く、`image_gap: 12`。以前の `adapt` + `contain` は 4:3 と 3:4 が混ざって空セルができ、スマホでは写真の上下に白い帯が出ていた。元写真は 939〜1170px しかないので `extend_media` や 1 列表示 (883px 幅) は拡大がかかるため使わない
- トップの商品グリッドは 3 列 (`templates/index.json`)。商品が 3 点なので 4 列だと右端が空く
- PayPal の黄色いボタン (`accelerated-checkout`) はテーマ設定では消せない (静的ブロックで JSON から外しても描画される)。消すなら管理画面の「決済」で PayPal Express を無効にする。未対応
- 残課題 (写真の追加が要る): 初回セットは 3 枚なので 2×2 の 4 セル目が空、5kg と 1 年契約は 2 枚で左列の下が空く。各商品 4 枚 (炊き上がり / 袋 / 米粒 / 田んぼ or 店) に揃えると埋まる。候補は `~/work/najilaboule-tmp/03_kama/` (羽釜。6000px 級)

## 商品 (2026-09-24 登録)

2026 年度販売計画 (`~/work/najilaboule-tmp/2026年度販売計画.pdf`) の 3 プランを商品として登録した (Admin API の `productCreate`。テーマではなくストアのデータなので、このリポジトリには入らない)。

| プラン | ハンドル | 商品 ID | 税込価格 | SKU |
|---|---|---|---|---|
| 初回販売 (〜11/30。新之助 5kg ＋ きたりえカレー 2 個) | `shinnosuke-5kg-kitarie-curry-set` | 7982710358067 | 6,480 円 | NJ-SHIN-5KG-CURRY |
| 新春セール・1 年契約 (12/1〜12/31。5kg × 12 袋、2 か月分無料) | `shinnosuke-5kg-12bags-annual` | 7982710456371 | 64,800 円 | NJ-SHIN-5KG-12 |
| レギュラー (12/1〜。5kg) | `shinnosuke-5kg` | 7982710554675 | 6,480 円 | NJ-SHIN-5KG |

- ストアは税込表示 (`shop.taxesIncluded = true`) なので、計画の税抜 6,000 円 / 60,000 円に軽減税率 8% を乗せた税込価格で登録した。ストア側の税率設定 (8% の上書き) は未確認
- 1 年契約は定期購入アプリを使わず、12 袋分を一括払いする通常商品として登録 (毎月の発送は運用で行う)
- 申込期間の開始・終了は自動化していない。3 商品とも公開中 (ストアはパスワード保護中)。12/1 公開にするなら管理画面の「公開日時を設定」か `publishablePublish` の `publishDate`
- 在庫は追跡しない (`tracked: false`)。送料込み (沖縄・離島 +1,000 円) は配送設定側で行う (未設定)
- 画像は `~/work/najilaboule-tmp/07_rice` `08_curry` から。商品カードの縦横比 (`image_ratio: adapt`) を揃えるため、主画像は 4:3 に切り抜いた
- 旧テスト商品 3 点 (`銀座 Naji la boule の米 ― ギフト 300g / お試し 1kg / 家庭用 5kg`、2026-05-26 作成) は同日ユーザー指示で削除した。商品はこの 3 点だけ

## 開発フロー

- 設定を変えたら: `shopify theme push --store vuvwb5-6g.myshopify.com --theme 145592877107 --only <変えたファイル>` (例: `--only config/settings_data.json --only templates/index.json`)
- テーマエディタで変えたら: `shopify theme pull --store vuvwb5-6g.myshopify.com --theme 145592877107` で取り込んでコミット
- プレビュー: `shopify theme dev --store vuvwb5-6g.myshopify.com` (http://127.0.0.1:9292) または管理画面のテーマ プレビュー (ストアはパスワード保護中)
- push 先は公開中テーマ (Horizon) なので、`shopify theme push` の前に必ず `pull` して差分を確認する。`--only` で対象ファイルを絞る
- コミットしたら `main` に取り込み (fast-forward) GitHub に push するところまで進める (2026-09-24 ユーザー承認。毎回の確認は不要)
