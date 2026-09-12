# Setup — getting this fully live

## 1. Create a Supabase project (free)
1. Go to supabase.com → sign up → "New Project"
2. Name it, set a database password, pick a region near India (Singapore)
3. Once created, go to Project Settings → API and copy:
   - Project URL
   - anon public key

## 2. Create the database table
In Supabase, open the SQL Editor and run:

```sql
create table kv_store (
  key text primary key,
  value jsonb not null,
  updated_at timestamptz default now()
);

alter table kv_store enable row level security;

create policy "Public read access" on kv_store
  for select using (true);

create policy "Public write access" on kv_store
  for insert with check (true);

create policy "Public update access" on kv_store
  for update using (true);

create policy "Public delete access" on kv_store
  for delete using (true);
```

## 3. Plug your keys into index.html
Open `index.html`, find this block near the bottom script section:

```js
const SUPABASE_URL = 'YOUR_SUPABASE_PROJECT_URL_HERE';
const SUPABASE_ANON_KEY = 'YOUR_SUPABASE_ANON_KEY_HERE';
```

Replace both placeholder strings with your real values from step 1.

## 4. Push to GitHub
```
git init
git add .
git commit -m "Initial Prince Builders site"
git branch -M main
git remote add origin <your-empty-github-repo-url>
git push -u origin main
```

## 5. Deploy
Easiest free option: Vercel or Netlify — connect your GitHub repo, no build settings needed (it's static HTML), deploy. You'll get a live URL in under a minute.

Domain: point your registered domain (Hostinger or wherever you bought it) at Vercel/Netlify following their "custom domain" instructions — this is just a DNS change, a few minutes of setup.

## Known limitation
The Developer panel's PIN gate (`1234`) is a front-end-only UI lock, not real security — anyone who views the page source can see it. Fine for an internal demo; replace with real authentication (Supabase Auth) before giving real people edit access in production.
