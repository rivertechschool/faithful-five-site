# Faithful Five Site — Deployment & DNS

## 1. Where it lives

- **GitHub repo:** [rivertechschool/faithful-five-site](https://github.com/rivertechschool/faithful-five-site)
- **GitHub Pages URL (always works):** https://rivertechschool.github.io/faithful-five-site/
- **Custom domain (after DNS):** https://faithfulfive.org
- **Hosting:** GitHub Pages (free, no server)

## 2. Namecheap DNS setup

Log into Namecheap → **Domain List** → click **Manage** next to `faithfulfive.org` → **Advanced DNS** tab.

### Remove first

Delete any existing records for the `@` host or `www` host that conflict (Namecheap parking pages, URL Redirect records, default CNAME, etc.). If you see a "URL Redirect Record" pointing the bare domain to a Namecheap page, delete it.

### Add these 5 records

| Type | Host | Value | TTL |
|------|------|-------|-----|
| A Record | `@` | `185.199.108.153` | Automatic |
| A Record | `@` | `185.199.109.153` | Automatic |
| A Record | `@` | `185.199.110.153` | Automatic |
| A Record | `@` | `185.199.111.153` | Automatic |
| CNAME Record | `www` | `rivertechschool.github.io.` | Automatic |

**Notes:**
- The four A records all use host `@` — that routes the bare domain `faithfulfive.org` to GitHub Pages.
- The CNAME routes `www.faithfulfive.org` to GitHub Pages and GitHub handles the rest via the `CNAME` file in the repo.
- Do **not** include `https://` or any path in the CNAME value. The trailing dot is optional — Namecheap accepts either form.
- TTL "Automatic" is fine (Namecheap default is 30 min).

### Save

Click the green checkmark to save each record. Namecheap will show a "DNS setup may take 30 minutes" banner — in practice it usually propagates in 5–15 minutes.

## 3. Verify DNS is working

Once records are saved, from any terminal:

```
dig +short faithfulfive.org
dig +short www.faithfulfive.org
```

Expected output:
- `faithfulfive.org` → four lines starting with `185.199.10…`
- `www.faithfulfive.org` → `rivertechschool.github.io.` followed by GitHub's IPs

Or just visit https://faithfulfive.org in your browser.

## 4. Enable HTTPS on GitHub

After DNS resolves:

1. Go to https://github.com/rivertechschool/faithful-five-site/settings/pages
2. You'll see "DNS check successful" next to `faithfulfive.org`
3. Check the **Enforce HTTPS** box. GitHub provisions a Let's Encrypt cert automatically (usually within a few minutes, sometimes up to 24 hours for a brand-new domain).

## 5. Making updates

Edit files locally, commit, push — Pages rebuilds in about 30–60 seconds.

```
git add .
git commit -m "describe change"
git push
```

Or edit directly in the GitHub web UI for small text fixes.

## 6. Troubleshooting

- **"Domain's DNS record could not be retrieved" in Pages settings** — DNS hasn't propagated yet. Wait 15–30 minutes and click "Check again."
- **HTTPS padlock broken after enabling** — The Let's Encrypt cert is still provisioning. Wait up to an hour. Uncheck and re-check "Enforce HTTPS" to retry if it gets stuck.
- **Old Namecheap parking page still showing** — Browser cache. Try an incognito window or `curl -I https://faithfulfive.org`.

## 7. What's deployed

- `index.html` — Home
- `pages/story.html` — Our Story (founding narrative, mandate, timeline)
- `pages/ministries.html` — Jesus People Coffee House + River Tech School + partners
- `pages/support.html` — Zeffy CTA, mail-check address, donor fact table
- `pages/contact.html` — Contact info + routing table
- `404.html` — Custom 404
- `sitemap.xml` — For search engines
- `robots.txt`
- `CNAME` — `faithfulfive.org` (tells Pages which custom domain)
- `assets/` — CSS, logo, favicons, OG image

## 8. Sibling sites for reference

- [rivertechschool/river-tech-site](https://github.com/rivertechschool/river-tech-site) → rivertechschool.com (red)
- [rivertechschool/rivertech-llc-site](https://github.com/rivertechschool/rivertech-llc-site) → rivertech.llc (blue)
- This repo → faithfulfive.org (forest green)
