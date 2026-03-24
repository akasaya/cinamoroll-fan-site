# TODO

## 次にやること

### 1. Supabase DBテーブル作成（未実施）

Supabase Dashboard → SQL Editor に以下を貼り付けて実行する。

```sql
-- カテゴリ
CREATE TABLE categories (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  name text NOT NULL UNIQUE,
  slug text NOT NULL UNIQUE,
  sort_order integer
);

-- グッズ
CREATE TABLE goods (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  name text NOT NULL,
  description text,
  price integer,
  release_date date NOT NULL,
  category_id uuid REFERENCES categories(id),
  status text NOT NULL DEFAULT 'active',
  image_url text,
  created_at timestamptz DEFAULT now(),
  updated_at timestamptz DEFAULT now()
);

-- 販売店リンク
CREATE TABLE stores (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  goods_id uuid REFERENCES goods(id) ON DELETE CASCADE,
  store_name text NOT NULL,
  store_type text NOT NULL,
  url text NOT NULL,
  is_active boolean DEFAULT true
);

-- ツイート参照
CREATE TABLE tweet_refs (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  tweet_id text NOT NULL UNIQUE,
  tweeted_at timestamptz NOT NULL,
  description text,
  is_featured boolean DEFAULT false,
  created_at timestamptz DEFAULT now()
);

-- RLS有効化
ALTER TABLE categories ENABLE ROW LEVEL SECURITY;
ALTER TABLE goods ENABLE ROW LEVEL SECURITY;
ALTER TABLE stores ENABLE ROW LEVEL SECURITY;
ALTER TABLE tweet_refs ENABLE ROW LEVEL SECURITY;

-- 全員が読み取り可能
CREATE POLICY "public read" ON categories FOR SELECT USING (true);
CREATE POLICY "public read" ON goods FOR SELECT USING (true);
CREATE POLICY "public read" ON stores FOR SELECT USING (true);
CREATE POLICY "public read" ON tweet_refs FOR SELECT USING (true);

-- カテゴリ初期データ
INSERT INTO categories (name, slug, sort_order) VALUES
  ('ぬいぐるみ', 'plush', 1),
  ('文具', 'stationery', 2),
  ('食器', 'tableware', 3),
  ('アパレル', 'apparel', 4),
  ('コラボ', 'collab', 5),
  ('その他', 'other', 6);
```

### 2. 環境変数を Cloudflare Pages に設定

Supabase → Settings → API から取得して Cloudflare Pages の環境変数に追加する。

| 変数名 | 取得場所 |
|--------|---------|
| `PUBLIC_SUPABASE_URL` | Supabase > Settings > API > Project URL |
| `PUBLIC_SUPABASE_ANON_KEY` | Supabase > Settings > API > anon public |

### 3. Astro に Supabase を接続

```bash
npm install @supabase/supabase-js
```

### 4. ページ作成

- トップページ（最新グッズ表示）
- グッズ一覧ページ
- ツイート埋め込みページ
