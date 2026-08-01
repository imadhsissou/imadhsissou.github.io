# Imad's Astrophotography Journal

A simple, APOD-inspired log of my astrophotography — one photo + a few notes per
entry — with a modern gallery homepage. Built with **Jekyll** and hosted on
**GitHub Pages**. New entries are posted through an in-browser editor
(**Sveltia CMS**) at `/admin/`.

```
├── index.html            Homepage: latest capture + gallery of the whole log
├── about.md              About page
├── _posts/               One markdown file = one astrophoto entry
├── assets/
│   ├── css/style.css     The whole look (dark, minimal)
│   └── photos/           Your images live here
├── admin/                The in-browser editor (Sveltia CMS)
│   ├── index.html
│   └── config.yml
└── _layouts / _includes  Page templates
```

---

## Posting a new photo

Once the one-time setup below is done, publishing is a 4-field form:

1. Go to **`https://imadhsissou.github.io/admin/`**
2. Log in with GitHub.
3. **New Entry** → title, date, drag in the photo, write your notes.
4. **Publish.** It commits to the repo; the site rebuilds in ~1 minute.

No editor set up yet? You can always add a file to `_posts/` directly on
GitHub — copy `_posts/2026-08-01-first-light.md` as a template.

---

## One-time setup for the `/admin/` editor

GitHub Pages is static, so the editor needs a tiny (free) helper to log you in
with GitHub. Two steps, ~10 minutes, done once.

### 1. Create a GitHub OAuth App
- GitHub → **Settings → Developer settings → OAuth Apps → New OAuth App**
- **Homepage URL:** `https://imadhsissou.github.io`
- **Authorization callback URL:** `https://YOUR-WORKER.workers.dev/callback`
  (you'll get this URL in step 2 — you can edit it afterward)
- Save the **Client ID** and generate a **Client Secret**.

### 2. Deploy the free auth relay (Cloudflare Worker)
- Use the ready-made worker: **https://github.com/sveltia/sveltia-cms-auth**
- Deploy it to Cloudflare (free tier), setting the environment variables
  `GITHUB_CLIENT_ID` and `GITHUB_CLIENT_SECRET` from step 1.
- Copy the Worker URL (e.g. `https://sveltia-cms-auth.you.workers.dev`).
- Back in the GitHub OAuth App, set the callback URL to `<worker-url>/callback`.

### 3. Point the CMS at it
- In [`admin/config.yml`](admin/config.yml), uncomment and set:
  ```yaml
  backend:
    name: github
    repo: imadhsissou/imadhsissou.github.io
    branch: master
    base_url: https://sveltia-cms-auth.you.workers.dev
  ```
- Commit. Visit `/admin/` and log in. Done.

> Full walkthrough: the Sveltia CMS docs cover this exact GitHub Pages flow.
> I'm happy to help wire it up — just ask.

---

## Editing locally (optional)

```bash
bundle install                 # first time only
bundle exec jekyll serve       # http://localhost:4000
```

To use the editor locally without the OAuth relay, set `local_backend: true`
in `admin/config.yml` and run `npx @sveltia/cms-proxy-server` in another terminal.

---

## Customizing

- **Colors / fonts:** the CSS variables at the top of `assets/css/style.css`.
- **Site title, social links, author:** `_config.yml`.
- **Homepage layout:** `index.html`. **Entry layout:** `_layouts/entry.html`.
