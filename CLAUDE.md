# Najilaboule Shop (Shopify Theme)

自社の和食店ブランド「Naji la boule」のお米を販売する EC サイト用 Shopify テーマ。
ベースは Shopify 公式テーマ **Horizon 4.1.5** (Theme Store 版)。LP (https://atimot.github.io/najilaboule/) の世界観のうち **色だけ** を継承する (フォントは 2026-09-24 から LP と別。下記)。LP の構造 (店舗紹介・予約導線) は継承しない。

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

色の正 (source of truth) は LP リポジトリの `/Users/tomitad/work/najilaboule/DESIGN.md`。

**触ってよいのはテーマ設定だけ** = `config/settings_data.json` の `current` (テーマエディタ「テーマ設定」と同じもの)。

**カスタム CSS (`platform_customizations.custom_css`) は使わない** (2026-09-24 決定)。テーマ設定の値を上書きして管理画面の設定が効かなくなるため。テーマ設定で表現できない見た目は、テーマ設定の範囲で近いものに寄せる。

`sections/` `blocks/` `snippets/` `assets/` `layout/` の Liquid / CSS / JS 本体は **編集しない** (Theme Store の更新に追従できなくなるため)。

**トークン対応 (DESIGN.md → Horizon カラーパレット):**

| パレットキー | 値 | DESIGN.md |
|---|---|---|
| background | #241816 | brand |
| foreground | #f8f8f8 | text |
| accent | #c8a67b | accent (金。セールバッジのみ) |
| muted | #99a1af | text-muted |
| surface | #2a1d1b | brand-light (filled ボタンの地) |
| color1 | #1f1513 | brand-dark (フッター背景) |
| color2 | #504645 | line (白 20% を brand に重ねた実色) |

- フォント: 本文 / 小見出し / 見出し / アクセントの 4 か所すべてシステムフォントの `serif` (Web フォントを読み込まない)。参考サイト (八代目儀兵衛 銀座米料亭) の游明朝はライブラリに無いため、端末の標準明朝で代える。和文は Mac・iPhone ならヒラギノ明朝。欧文は Shopify のシステム serif 指定 (New York → Iowan Old Style → … → Times New Roman) で決まる
- 字間: 見出し h1〜h6 は「広め」(0.03em)。本文の字間は設定が無いので標準のまま
- 行間: 本文は「広め」(1.6)
- 角丸はすべて 0、影なし、カードのホバー効果なし
- ボタン: primary = 透明地・白文字・白 50% 枠 (`rgba(255,255,255,0.5)`)、secondary = 白 5% 地・line 枠。ホバー色は Horizon の自動計算に任せる

## 開発フロー

- 設定を変えたら: `shopify theme push --store vuvwb5-6g.myshopify.com --theme 145592877107 --only config/settings_data.json`
- テーマエディタで変えたら: `shopify theme pull --store vuvwb5-6g.myshopify.com --theme 145592877107` で取り込んでコミット
- プレビュー: `shopify theme dev --store vuvwb5-6g.myshopify.com` (http://127.0.0.1:9292) または管理画面のテーマ プレビュー (ストアはパスワード保護中)
- push 先は公開中テーマ (Horizon) なので、`shopify theme push` の前に必ず `pull` して差分を確認する。`--only` で対象ファイルを絞る
