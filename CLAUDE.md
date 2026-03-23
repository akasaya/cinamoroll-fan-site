# cinamoroll-fan-site

シナモロール非公式ファンサイト。公式ツイート埋め込みと新作グッズリンクをまとめる。

## プロジェクト概要

- **スタック**: Astro（静的）+ Cloudflare Pages + Supabase
- **リポジトリ**: https://github.com/akasaya/cinamoroll-fan-site
- **著作権方針**: 画像転載なし・X公式埋め込みのみ・全ページに非公式表記

## スタック詳細

| レイヤー | 技術 | 備考 |
|---------|------|------|
| フロントエンド | Astro（`output: 'static'`） | アダプター不要 |
| ホスティング | Cloudflare Pages | 帯域無制限・無料 |
| DB / Auth / Storage | Supabase | 無料枠（500MB・50,000 MAU） |
| CI | GitHub Actions | `.github/workflows/ci.yml` |
| 依存関係更新 | Dependabot | 週次・月曜日 |

## Cloudflare Pages 設定

```toml
# wrangler.toml
name = "cinamoroll-fan-site"
compatibility_date = "2024-01-01"
pages_build_output_dir = "dist"
```

- ビルドコマンド: `npm run build`
- 出力ディレクトリ: `dist/`
- Node.js バージョン: 22

## 既知のエラーと解決策

### wrangler.toml が見つからないエラー
```
Could not find a wrangler.json, wrangler.jsonc, or wrangler.toml file
```
→ `wrangler.toml` をリポジトリルートに追加（解決済み）

### Workers タブと Pages タブの混同
- `Deploy command: npx wrangler deploy` が出たら Workers 側にいる
- → Cloudflare Dashboard: **Pages タブ → Connect to Git** を選ぶ

### Astro アダプターとの競合
- `output: 'static'` のとき `@astrojs/cloudflare` を設定するとビルドエラー
- → `astro.config.mjs` にアダプターは設定しない（解決済み）

## 開発ルール

- Cloudflare 関連の作業は `cloudflare-pages-deploy` スキルを参照すること
- 月額 $0 を維持する（全サービス無料枠内で収める）
- PRを出すとCloudflare PagesがプレビューURLを自動生成する
