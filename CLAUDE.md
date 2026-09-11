# Najilaboule Shop (Shopify Theme)

銀座の和食店「Naji la boule」のお米を販売する Shopify テーマ。LP の世界観を継承する。

- ストア: `najilaboule.myshopify.com`
- 公開中テーマ: Dawn (ID: 143036547123)
- ベース: Shopify Dawn テーマをカスタマイズ

## デザイン決め事 (重要)

このプロジェクトは姉妹プロジェクトの LP (`/Users/tomitad/work/najilaboule`) と **デザイン統一** をしている。**このテーマの正は直下の [`DESIGN.md`](./DESIGN.md)** (Google Labs の design.md オープン仕様。フロントマターのトークンが normative、本文が Dawn の設定値との対応)。LP 側の [`DESIGN.md`](https://github.com/atimot/najilaboule/blob/main/DESIGN.md) は上流で、リポジトリは統合しない (2026-09-12 決定)。

- 色・フォント・角丸・影を設定・変更する際は、まずこのリポジトリの `DESIGN.md` を参照し、変えたらフロントマターと `config/settings_data.json` を同じ値に揃える
- LP と同名のトークン (`primary` / `accent` / `brand` / `brand-dark` / `brand-light` / `text`) は LP と同じ値を保つ。最終同期は LP の DESIGN.md コミット `0fcf8b6` (2026-09-10)。LP 側が変わったら `git -C /Users/tomitad/work/najilaboule log 0fcf8b6..main -- DESIGN.md` で差分を見る
- 検証: `npx -y @google/design.md@0.4.0 lint DESIGN.md` (エラー 0・警告 0 を維持)。drift 検出: `npx -y @google/design.md@0.4.0 diff /Users/tomitad/work/najilaboule/DESIGN.md DESIGN.md` で `colors` の `modified` が空であること (`added` / `removed` は両プロジェクト固有のトークンなので出てよい)
- LP の構造（店舗紹介・予約導線等）は **継承しない**。あくまで世界観 (色・タイポ・コピー語感) のみ寄せる
- **同期する範囲は `config/settings_data.json` の設定値のみ** (2026-09-11 決定): 色スキーム、フォント (`zen_old_mincho_n4`、和文・欧文とも)、角丸 0、影 0。LP の余白・行間・字間・モーション・背景 3 層・フォーカスリング等の CSS 層は **追従しない** (EC の情報密度と Dawn の保守性を優先)。CSS / Liquid の追記による寄せは提案しないこと
- EC としての意図的な例外 (詳細は `DESIGN.md` の Do's and Don'ts): 購入系 CTA (カートに追加・購入・送信) は Dawn の primary ボタンのまま金 (`#c8a67b`) 塗りでよい (LP の One Gold Rule は適用しない)。カート・検索等の SVG アイコンは残す。商品写真は減光しない
- `presets.Dawn` ブロックは Dawn のリセット用なので変更しない (`current` のみ更新)
- 2026-05-25 の初回寄せの経緯は `docs/superpowers/specs/2026-05-25-lp-design-alignment-design.md`

## 開発フロー

- ローカルプレビュー: `shopify theme dev --store najilaboule.myshopify.com` (http://127.0.0.1:9292)
- 開発テーマは自動で作成され、Live の Dawn テーマには影響しない (Shopify は7日後に自動削除)
- 公開中テーマへ直接 push しない。`--unpublished` フラグや明示的なテーマID指定で安全運用
- ストアの永続ドメインは `vuvwb5-6g.myshopify.com` (`najilaboule.myshopify.com` はプライマリドメインの別名)。`shopify store auth` / `shopify store execute` は永続ドメインでないと失敗する。`.claude/settings.json` の `env.SHOPIFY_FLAG_STORE` で Claude Code の Bash には自動で渡る
- Shopify Dev MCP (`@shopify/dev-mcp`) は `.mcp.json` に登録済み。初回セッションで承認が必要
