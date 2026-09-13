# Deploying Otter Work to GitHub Pages

The site is a static site (no build step). Deploying means pushing the files to
`github.com/IgnasSavTenging/otterwork-pages` and turning on GitHub Pages.

---

## 1. Push the site

From the repo root (`/Users/ignassavickas/projects/otterwork-pages`):

```bash
git add -A
git commit -m "Otter Work marketing site (Depot design)"
git push -u origin main
```

If the default branch is `master` instead of `main`, use `master` consistently below.

---

## 2. Enable GitHub Pages

There are two ways. **Deploy from a branch** is simplest for a plain static site.

### Option A — Deploy from a branch (recommended here)

1. Go to **github.com/IgnasSavTenging/otterwork-pages → Settings → Pages**.
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Set **Branch** to `main` and **folder** to `/ (root)`. Click **Save**.
4. Wait ~1–2 minutes. Pages publishes the site and shows the live URL
   (`https://ignassavtenging.github.io/otterwork-pages/`).

The repo already contains a `.nojekyll` file, so GitHub serves the files as-is
without running Jekyll.

### Option B — GitHub Actions workflow

If you prefer Actions (useful once a build step is added), set **Source** to
**GitHub Actions** and add `.github/workflows/pages.yml`. Not needed for the
current static site — Option A is enough. Ask if you want this generated.

---

## 3. Verify

- Visit `https://ignassavtenging.github.io/otterwork-pages/` and confirm the page loads.
- Once the custom domain is set (next section), the canonical URL becomes
  `https://otterwork.app`.

---

## 4. Map the custom domain `otterwork.app`

The repo already ships a `CNAME` file containing `otterwork.app`, so GitHub will
pick up the custom domain on the next deploy. You still need DNS records at your
domain registrar/DNS host for `otterwork.app`.

### 4a. Tell GitHub about the domain

1. **Settings → Pages → Custom domain**: enter `otterwork.app`, click **Save**.
   (This is already encoded by the `CNAME` file; entering it in the UI triggers
   verification.)
2. Leave **Enforce HTTPS** unchecked *for now* — you can only enable it after the
   TLS certificate is issued (step 4d).

### 4b. Add DNS records at your DNS provider

`otterwork.app` is an **apex/root domain**, so use **A + AAAA records** pointing to
GitHub Pages' IPs. Add all four A records and all four AAAA records:

**A records** (host `@`):
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

**AAAA records** (host `@`):
```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

**Also add the `www` subdomain** as a CNAME so `www.otterwork.app` redirects to the
apex:
```
Type: CNAME   Host: www   Value: ignassavtenging.github.io
```

> Note: `.app` is a Google-operated TLD on the **HSTS preload list**, so it *requires*
> HTTPS — the site will not load over plain HTTP. GitHub Pages issues a free
> Let's Encrypt certificate automatically (step 4d), which satisfies this.

### 4c. (Recommended) Verify the domain to prevent takeovers

**GitHub → Settings (your account) → Pages → Add a domain** and follow the
`TXT` record instructions (`_github-pages-challenge-...`). This binds the domain to
your account.

### 4d. Wait for DNS + certificate

- DNS propagation: minutes to a few hours.
- Back in **repo Settings → Pages**, GitHub shows "DNS check successful" once the
  records resolve, then provisions the HTTPS certificate (can take up to ~24h, but
  usually much faster).
- When the cert is ready, tick **Enforce HTTPS**.

Check propagation from the terminal:

```bash
dig +short otterwork.app
dig +short AAAA otterwork.app
dig +short www.otterwork.app
```

You should see the four GitHub IPs (and IPv6 addresses), and `www` resolving to
`ignassavtenging.github.io`.

---

## 5. Updating the site later

Edit files, then:

```bash
git add -A
git commit -m "Update marketing copy"
git push
```

GitHub Pages redeploys automatically within a minute or two. Do **not** delete the
`CNAME` file — removing it unsets the custom domain.
