# Najilaboule Shop (Shopify Theme)

銀座の和食店「Naji la boule」のお米を販売する Shopify テーマ。LP の世界観を継承する。

- ストア: `najilaboule.myshopify.com`
- 公開中テーマ: Dawn (ID: 143036547123)
- ベース: Shopify Dawn テーマをカスタマイズ

## デザイン決め事 (重要)

このプロジェクトは姉妹プロジェクトの LP (`/Users/tomitad/work/najilaboule`) と **デザイン統一** をしている。デザイントークン・タイポ・余白・コピー語感の **正 (source of truth)** は LP プロジェクト直下の [`/Users/tomitad/work/najilaboule/DESIGN.md`](/Users/tomitad/work/najilaboule/DESIGN.md) に集約。

- 色・フォント・余白を設定・変更する際は、必ず `DESIGN.md` を参照する
- LP の構造（店舗紹介・予約導線等）は **継承しない**。あくまで世界観 (色・タイポ・余白・コピー語感) のみ寄せる
- `config/settings_data.json` の色・タイポ設定は `DESIGN.md` の「Shopifyへの翻訳」セクションを参考にする

## 開発フロー

- ローカルプレビュー: `shopify theme dev --store najilaboule.myshopify.com` (http://127.0.0.1:9292)
- 開発テーマは自動で作成され、Live の Dawn テーマには影響しない (Shopify は7日後に自動削除)
- 公開中テーマへ直接 push しない。`--unpublished` フラグや明示的なテーマID指定で安全運用
