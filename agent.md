# Agent Instructions: cinamoroll-fan-site

シナモロール非公式ファンサイトの開発エージェント向け設定。

## プロジェクト概要

- **目的**: シナモロールの公式ツイート埋め込みと新作グッズリンクをまとめる非公式ファンサイト
- **スタック**: Astro（静的サイト）+ Cloudflare Pages + Supabase
- **リポジトリ**: https://github.com/akasaya/cinamoroll-fan-site

## 使用するスキル

### cloudflare-pages-deploy（必須）

Cloudflare Pages へのデプロイ作業では必ず `cloudflare-pages-deploy` スキルを参照すること。

**このプロジェクト固有の設定**:
```toml
# wrangler.toml
name = "cinamoroll-fan-site"
compatibility_date = "2024-01-01"
pages_build_output_dir = "dist"
```

**ビルド設定**:
- コマンド: `npm run build`
- 出力: `dist/`
- Node.js: 22
- アダプター: 不要（`output: 'static'`）

**既知のエラー（このプロジェクトで実際に発生）**:

1. `Could not find a wrangler.json, wrangler.jsonc, or wrangler.toml file`
   - → `wrangler.toml` を追加して解決済み

2. Workers タブ vs Pages タブの混同
   - Cloudflare Dashboard で Workers タブを選ぶと `Deploy command: npx wrangler deploy` が表示される
   - → Pages タブ → Connect to Git を選ぶこと

3. `@astrojs/cloudflare` アダプターとの競合
   - 静的サイトにアダプターを設定するとビルドエラー
   - → `astro.config.mjs` からアダプター設定を削除して解決済み

## 開発ルール

- 著作権遵守: 画像転載なし、X公式埋め込みのみ
- 全ページに「非公式ファンサイト」を明記
- 月額 $0 維持（全サービス無料枠内）

## CI/CD

- **GitHub Actions**: `.github/workflows/ci.yml`（型チェック・ビルド自動実行）
- **Dependabot**: `.github/dependabot.yml`（週次npm更新PR）
- **Cloudflare Pages**: mainブランチPush時に自動デプロイ
- **プレビュー**: PR作成時にプレビューURLが自動生成される
