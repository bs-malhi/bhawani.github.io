# GS Visibility: site source (Eleventy + Decap CMS)

Netlify runs `npm run build` and publishes `_site`. Everything is configured in `netlify.toml`.

## Where things live
- `src/_data/site.json`: phone, email, WhatsApp link, GA4 ID, Search Console tag, service list. Change a detail here once and it updates everywhere.
- `src/_includes/`: shared layouts (`base.njk` holds the head, hreflang, schema wiring, header, footer, cookie banner).
- `src/*.njk`, `src/services/`, `src/blog/`: the pages.
- `src/admin/`: the blog editor (Decap CMS) at /admin/.
- `brand-assets/`: original logo files and a 1000px social profile picture. Not deployed.

## Launch order (do it in this order, so nothing goes offline)
1. **Netlify, Domain management:** add `gsvisibility.com` as a custom domain on this site. Netlify adds `www` automatically; make `gsvisibility.com` the primary domain.
2. **DNS where the domain is registered (leave the MX/email records alone):**
   - apex `@`: A record to `75.2.60.5` (or ALIAS/ANAME to `apex-loadbalancer.netlify.com` if your DNS provider offers it)
   - `www`: CNAME to `<your-site-name>.netlify.app`
   Wait until Netlify shows the domain as verified and HTTPS is issued.
3. **GitHub:** upload these files to the repo (see the note below), commit to the default branch. Netlify builds automatically.
4. **Netlify, remove old snippet injection:** Site configuration > Build & deploy > Post processing > Snippet injection. Delete the old GA4 and Search Console snippets. GA4 is now loaded by the site itself, only after a visitor accepts cookies; leaving the old snippets would load it twice and bypass consent.
5. **Search Console:** add a **Domain** property for `gsvisibility.com` (verify with the DNS TXT record Google gives you), then submit `https://gsvisibility.com/sitemap.xml`. The page-level verification tag is already in the site.
6. **GA4:** Admin > Data streams > edit the web stream URL to `https://gsvisibility.com`. In Events, mark `generate_lead` as a key event (it fires on the thank-you page).
7. **Bing Webmaster Tools:** import the site from Search Console.
8. **Blog editor:** Netlify > Identity > Enable, Registration = Invite only, Services > Git Gateway > Enable, then invite yourself. Check `branch:` in `src/admin/config.yml` matches your repo's default branch.

## Uploading to GitHub without deleting your posts
Upload the contents of this folder over the repo. Files with the same name are replaced; files only you have (for example blog posts you published from /admin/ in `src/blog/` and images in `src/assets/blog/`) stay. Delete the old root-level files that no longer exist here, such as an old `index.html`, if any remain.

## International SEO setup (what is built in)
- English-language site for Europe and Australia, British/Australian spelling, `lang="en-GB"`.
- Every indexable page outputs a self-referencing `hreflang="en"` plus `x-default`. If regional pages are added later (for example `/au/`), list them in that page's front matter under `hreflangs` and every page in the cluster will output reciprocal tags.
- Organisation/WebSite/Service/BlogPosting/FAQ/Breadcrumb structured data with `areaServed` set to Europe and Australia.
- Canonical host is `https://gsvisibility.com`; `www` and the old `bhawanigrowth.netlify.app` address 301 to it (see `netlify.toml`).
- Fonts are self-hosted (no Google Fonts request, relevant for GDPR); analytics load only after consent.
