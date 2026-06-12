# Deploy & one-time setup

A checklist for taking Inner Circle live. Most steps are one-time. Claude can do
the Git/commit parts; the account-creation and authorization steps must be done by
you (entering passwords, granting OAuth, and changing account settings are things
Claude won't do on your behalf).

## 1. GitHub (storage + version history)

1. Create a repository (private is fine), e.g. `inner-circle`.
2. Push this `site/` folder to it:
   ```bash
   git remote add origin git@github.com:<you>/inner-circle.git
   git push -u origin main
   ```
3. In `static/admin/config.yml`, set `repo: <you>/inner-circle`.

## 2. Netlify (build + hosting)

1. Log in to Netlify with GitHub → **Add new site → Import an existing project** →
   pick the repo.
2. Netlify reads `netlify.toml` automatically (build = `hugo --gc --minify`,
   publish = `public`, Hugo 0.163.1). Click **Deploy**.
3. The site goes live at `https://<name>.netlify.app`. Rename under
   **Site configuration → Site details** if desired.

## 3. Custom domain (optional)

Netlify → **Domain management → Add a domain**. Point your registrar's DNS at
Netlify (or use Netlify DNS). HTTPS is automatic and free.

## 4. Automatic archive rotation (scheduled rebuild)

1. Netlify → **Site configuration → Build & deploy → Build hooks** → **Add build
   hook** (name it "Daily rebuild"). Copy the URL.
2. GitHub repo → **Settings → Secrets and variables → Actions → New repository
   secret** → name `NETLIFY_BUILD_HOOK`, value = the URL.
3. Done. `.github/workflows/scheduled-rebuild.yml` pings it daily at ~03:15 ET so
   past concerts move to the archive on their own. (Run it manually anytime from the
   repo's **Actions** tab → "Scheduled rebuild" → "Run workflow".)

## 5. Sveltia CMS login (`/admin`)

Sveltia uses its own hosted GitHub OAuth — **no server to run**. Visiting
`https://<your-site>/admin` and clicking "Sign in with GitHub" works once the repo
in `config.yml` is set and you authorize the app. (For a custom GitHub OAuth app
instead, see the Sveltia docs.)

## 6. Newsletter (Buttondown)

1. Create a free Buttondown account and note your username.
2. Set `buttondownUser = "<username>"` in `hugo.toml` under `[params]`.
3. The "Join the Inner Circle" form then posts subscribers straight to Buttondown.
   Until then, the section shows an email CTA instead.

---

### Quick reference

| Thing | Where |
|-------|-------|
| Add/edit a concert | `content/concerts/*.md` (or ask Claude, or `/admin`) |
| Design tokens | `assets/css/design-system.css` |
| Site settings (name, address, socials, newsletter) | `hugo.toml` → `[params]` |
| Home hero / featured override | `content/_index.md` |
| Build config | `netlify.toml` |
| Scheduled rebuild | `.github/workflows/scheduled-rebuild.yml` |
