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

## 開発フロー

- 設定を変えたら: `shopify theme push --store vuvwb5-6g.myshopify.com --theme 145592877107 --only <変えたファイル>` (例: `--only config/settings_data.json --only templates/index.json`)
- テーマエディタで変えたら: `shopify theme pull --store vuvwb5-6g.myshopify.com --theme 145592877107` で取り込んでコミット
- プレビュー: `shopify theme dev --store vuvwb5-6g.myshopify.com` (http://127.0.0.1:9292) または管理画面のテーマ プレビュー (ストアはパスワード保護中)
- push 先は公開中テーマ (Horizon) なので、`shopify theme push` の前に必ず `pull` して差分を確認する。`--only` で対象ファイルを絞る
- コミットしたら `main` に取り込み (fast-forward) GitHub に push するところまで進める (2026-09-24 ユーザー承認。毎回の確認は不要)
