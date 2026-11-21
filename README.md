# Bhawani Growth — site source

Built with Eleventy. Netlify runs `npm run build` and publishes `_site`.

## Structure
- `src/_includes/` — shared layouts (base.njk, service.njk, post.njk)
- `src/_data/site.json` — phone, email, WhatsApp link, etc. Edit this once to update them everywhere.
- `src/services/` — one file per service page
- `src/blog/` — blog index + one Markdown file per post
- `src/admin/` — the Decap CMS editor (see setup steps below)

## One-time setup (do this once, in your Netlify dashboard)
1. **Site settings → Identity → Enable Identity.**
2. Under Identity, open **Registration** and set it to **Invite only**.
3. Under Identity → **Services**, enable **Git Gateway** (this lets the CMS commit to your repo on your behalf).
4. Still under Identity, click **Invite users**, and invite your own email address. Accept the invite email — it'll ask you to set a password.
5. In **Site settings → Build & deploy**, confirm the build command is `npm run build` and the publish directory is `_site` (already set in `netlify.toml`, but double-check it matches).
6. Check `src/admin/config.yml` — the line `branch: main` must match your GitHub repo's actual default branch name. If your repo uses `master` instead, change that one line.

## Adding a blog post from here on
Go to `yoursite.com/admin/`, log in, click **New Blog Posts**, fill in the fields (title, date, author, tags, meta title, meta description, focus keyword, featured image, body), and click **Publish**. That commits a new Markdown file to GitHub, which triggers Netlify to rebuild and redeploy automatically — no coding involved after today.
