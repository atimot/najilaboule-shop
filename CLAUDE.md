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

**カスタム CSS は書体の 1 ルールだけ** (2026-09-24 決定)。`platform_customizations.custom_css` に `:root` の書体変数 4 つ (`--font-body--family` `--font-subheading--family` `--font-heading--family` `--font-accent--family`) を游明朝に上書きするルールがあり、これ以外は足さない。セクション単位 (テンプレート JSON 内の `custom_css`) も使わない。理由: カスタム CSS はテーマ設定の値を上書きして管理画面の設定が効かなくなる。書体だけは游明朝がライブラリに無いので例外にした。テーマ設定で表現できない見た目は、テーマ設定の範囲で近いものに寄せる。

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

- フォント: **游明朝** (参考サイト 八代目儀兵衛 銀座米料亭と同じ)。Web フォントは読み込まず、カスタム CSS で `"Yu Mincho", YuMincho, "Hiragino Mincho ProN", serif` を指定。Mac・Windows は游明朝、iPhone はヒラギノ明朝、Android は Noto Serif 系で表示される。テーマ設定の書体 4 か所は `serif` のままにしておく (CSS に上書きされるので値に意味は無いが、Web フォントを読み込ませないため)。ウェイトは 400 (Horizon の既定)
- 字間: 見出し h1〜h6 は「広め」(0.03em)。本文の字間は設定が無いので標準のまま
- 行間: 本文は「広め」(1.6)
- 角丸はすべて 0、影なし、カードのホバー効果なし
- ボタン: primary = 墨で塗る (地 foreground・文字 background・枠 foreground)、secondary = surface 地・foreground 文字・color2 枠。ホバー色は Horizon の自動計算に任せる

## 開発フロー

- 設定を変えたら: `shopify theme push --store vuvwb5-6g.myshopify.com --theme 145592877107 --only <変えたファイル>` (例: `--only config/settings_data.json --only templates/index.json`)
- テーマエディタで変えたら: `shopify theme pull --store vuvwb5-6g.myshopify.com --theme 145592877107` で取り込んでコミット
- プレビュー: `shopify theme dev --store vuvwb5-6g.myshopify.com` (http://127.0.0.1:9292) または管理画面のテーマ プレビュー (ストアはパスワード保護中)
- push 先は公開中テーマ (Horizon) なので、`shopify theme push` の前に必ず `pull` して差分を確認する。`--only` で対象ファイルを絞る
- コミットしたら `main` に取り込み (fast-forward) GitHub に push するところまで進める (2026-09-24 ユーザー承認。毎回の確認は不要)
