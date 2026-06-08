# Structure-Centric ML — Website

The reference website for the Structure-Centric Machine Learning paradigm.
Live at: **https://structurecentricml.com**

## Structure

```
.
├── index.html              # Home: paradigm, takeaway slide, two stacks
├── paradigm.html           # The structural reframe + lineage
├── low-d.html              # SCOPE → AdaBox → SLCD stack
├── high-d.html             # Graph-SCOPE → AdaGraph → DA-Sampler → SLCD stack
├── validation.html         # Genomics, Text, Materials cross-domain proof
├── roadmap.html            # 7-domain research program
├── publications.html       # Patents, arXiv, papers in preparation
├── about.html              # Researcher bio + 4 partnership types + contact
├── style.css               # Editorial design system
├── favicon.svg             # SC monogram favicon
├── CNAME                   # Custom-domain pointer for GitHub Pages
├── robots.txt              # Allow indexing
├── sitemap.xml             # Page index for search engines
├── data.json               # Verified benchmark numbers (single source of truth)
└── assets/curated/         # Real notebook figures + takeaway slide
    ├── hero_takeaway.png
    ├── text_*.png
    ├── graphscope_*.png
    ├── materials_*.png
    ├── genomics_*.png
    └── slcd_*.png
```

All figures are real outputs from your benchmark notebooks, not regenerated. Source notebook for each figure is cited in the `<figcaption class="src">` element below it.

---

## Deployment — Step by Step

### 1. Register the domain (Cloudflare Registrar)

The cheapest, no-upsell way to register `structurecentricml.com`:

1. Go to https://dash.cloudflare.com/sign-up and create a Cloudflare account.
2. Open the **Domain Registration** product (under "Account Home" → "Domain Registration → Register Domains").
3. Search for `structurecentricml.com`. It should be available for **$10.44/yr** at-cost (Cloudflare does not mark up domains).
4. Complete registration. Cloudflare automatically becomes your DNS provider.

> **Tip:** Optionally also register `sc-ml.org` as a backup (~$10/yr). You can redirect it to the main domain later.

### 2. Create the GitHub repository

1. Sign in to GitHub. Create a new repository — public is fine; private also works on GitHub Pro (~$4/mo).
2. Name suggestion: `structurecentricml` (the name doesn't matter, only the Pages settings do).
3. Do **not** initialize with a README (the deployment package includes its own).

### 3. Push the site files

From the directory containing this README (the deployment package):

```bash
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/<your-username>/structurecentricml.git
git push -u origin main
```

### 4. Enable GitHub Pages

1. In the repo, go to **Settings → Pages**.
2. Under **Source**, select **Deploy from a branch**.
3. Set **Branch** to `main` and folder to `/ (root)`. Click **Save**.
4. Wait ~30 seconds. GitHub will assign a URL like `https://<your-username>.github.io/structurecentricml/` and the site will go live there immediately.

### 5. Configure the custom domain in GitHub

1. Still in **Settings → Pages**, under **Custom domain**, enter `structurecentricml.com` and click **Save**.
2. Check the box **Enforce HTTPS** (it may take a few minutes to activate; come back later if it's greyed out).

The `CNAME` file in this repo already contains `structurecentricml.com`, which is what GitHub Pages needs to recognize the custom domain. You should see "DNS check in progress" in GitHub's UI — it will resolve once you do step 6.

### 6. Point Cloudflare DNS to GitHub Pages

In the Cloudflare dashboard, open `structurecentricml.com` → **DNS** → **Records**.

Add the following A records (for the apex domain) — these are GitHub Pages' published IPs:

| Type | Name | Value | Proxy |
|------|------|-------|-------|
| A    | @    | 185.199.108.153 | DNS only (gray cloud) |
| A    | @    | 185.199.109.153 | DNS only (gray cloud) |
| A    | @    | 185.199.110.153 | DNS only (gray cloud) |
| A    | @    | 185.199.111.153 | DNS only (gray cloud) |

Add a CNAME for the `www` subdomain:

| Type  | Name | Value | Proxy |
|-------|------|-------|-------|
| CNAME | www  | `<your-username>.github.io` | DNS only (gray cloud) |

> **Important:** Set Proxy status to **DNS only** (gray cloud icon), NOT **Proxied** (orange cloud). Cloudflare proxying conflicts with GitHub Pages' automatic SSL provisioning. Once GitHub has issued a Let's Encrypt certificate (visible in Settings → Pages), you can optionally re-enable Cloudflare proxying for performance.

### 7. Wait for propagation

DNS changes typically propagate in 5 minutes via Cloudflare. Test:

```bash
dig structurecentricml.com +short
# Should return the four 185.199.x.x addresses

curl -I https://structurecentricml.com
# Should return HTTP/2 200
```

Visit `https://structurecentricml.com` — the site should load.

If GitHub shows "DNS check unsuccessful," wait 10 minutes and click "Check again" in Settings → Pages.

### 8. Enable HTTPS

After DNS resolves, return to **Settings → Pages** and tick **Enforce HTTPS**. GitHub will auto-provision a Let's Encrypt certificate within ~15 minutes. After that, all traffic is forced to HTTPS.

---

## Total Cost Summary

| Item | Cost |
|------|------|
| Domain registration (Cloudflare Registrar) | **$10.44/year** |
| GitHub Pages hosting | Free |
| Cloudflare DNS | Free |
| SSL certificate (Let's Encrypt via GitHub) | Free |
| **Total** | **~$10–11/year** |

---

## Updating the site

To make changes:

```bash
# Edit any HTML file or the CSS
git add -A
git commit -m "Update copy on validation page"
git push
```

GitHub Pages re-deploys automatically within ~60 seconds.

To replace a benchmark figure with a newer notebook output, drop the new PNG into `assets/curated/` with the same filename. No code changes needed.

---

## Verified Numbers Single Source of Truth

All quantitative claims on the site reference the data in `data.json`, which was extracted directly from your nine benchmark notebooks. Key verified numbers:

- **Text benchmark (4 datasets, sentence-BERT + UMAP):**
  - AdaGraph avg ARI = **0.5015** (best); avg SCOPE = **0.7604**
  - AdaBox avg ARI = 0.4261; avg SCOPE = 0.5766
  - HDBSCAN* (tuned) avg ARI = 0.3611
  - HDBSCAN (default) avg ARI = 0.3516

- **20NG-6cat takeaway slide:**
  - HDBSCAN: k=64, 30.4% noise, ARI = 0.464
  - AdaGraph: k=7, 0% noise, ARI = 0.751

- **Graph-SCOPE synthetic benchmark:** 9 of 10 datasets won; 99.8% of oracle k-selection performance.

- **Graph-SCOPE dimension scaling:** τ = 0.95 at 5,000D where Silhouette collapses to 0.46.

- **Genomics — GSE31210 lung adenocarcinoma:** 24 wins / 3 ties / 13 losses combined scorecard.

- **Genomics — GSE14520 hepatocellular carcinoma:** 4 modules (1062 / 360 / 2048 / 6530 genes), 0% noise; WGCNA discarded 96.9% as background.

- **Materials — SuperCon (n=21,263, 81D):** AdaGraph found 18 superconductor families with 0% noise; HDBSCAN fragmented to 152 clusters with 40% noise.

- **SLCD Protocol A:** AdaBox Δ ARI = +0.027; DBSCAN Δ ARI = −0.404; HDBSCAN Δ ARI = −0.475.

If a number is challenged, the source notebook is cited under each figure on the validation page.

---

## Design System

The design language matches the takeaway slide:

- **Typography:** Fraunces serif (display + lede), Inter sans (body), JetBrains Mono (tags + numbers)
- **Palette:** paper `#F4EDE0`, ink `#1A1814`, accent `#B8412C` (warm red), navy `#2A4A6A`, good `#4A6B47`
- **Voice:** editorial-confident, no marketing fluff, every claim falsifiable

When extending the site, follow the patterns in existing pages — `kicker` italic + bold serif headline, `section-tag` mono, body in Inter, never `<img>` without a `<figcaption>` citing the source notebook.

---

© 2026 Ahmed Elmahdi, PhD · arXiv: 2605.16320
