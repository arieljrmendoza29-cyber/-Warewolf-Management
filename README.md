# WareWolf Management — Website

Single-page website for **WareWolf Management**, a Chicago-based full-cycle real estate services firm.

---

## 📁 Files in this folder

| File | Purpose |
|---|---|
| `index.html` | The entire website in one file (HTML + CSS + JS inline) |
| `hero-bg.mp4` | Hero section background video (~2.8 MB) |
| `logo.png` | Company logo (also embedded as base64 inside `index.html`) |
| `README.md` | This file |
| `.gitignore` | Git ignore rules |

That's it. No build step, no `npm install`, no framework — just static files.

---

## 🚀 Deploying to GitHub Pages

### 1. Create a new GitHub repository

Go to https://github.com/new and create a public repository. You can name it anything, but two common conventions are:

- **`warewolf-site`** (or any name) → site will live at `https://<your-username>.github.io/warewolf-site/`
- **`<your-username>.github.io`** → site will live at `https://<your-username>.github.io/` (root domain)

### 2. Upload these files

**Option A — drag and drop (easiest):**
1. Open your new repo on GitHub
2. Click **"Add file"** → **"Upload files"**
3. Drag all files from this folder (`index.html`, `hero-bg.mp4`, `logo.png`, `README.md`, `.gitignore`) into the upload area
4. Scroll down and click **"Commit changes"**

**Option B — command line:**

```bash
cd warewolf-site
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin main
```

### 3. Enable GitHub Pages

1. In your repo, go to **Settings** → **Pages** (left sidebar)
2. Under **"Source"**, select **"Deploy from a branch"**
3. Under **"Branch"**, select **`main`** and folder **`/ (root)`**
4. Click **Save**

After ~1–2 minutes, your site will be live at the URL shown on that page.

---

## 🌐 Using a custom domain (e.g. `warewolfmanagement.com`)

### 1. In your GitHub repo

- Go to **Settings → Pages**
- Under **"Custom domain"**, enter your domain (e.g., `warewolfmanagement.com`)
- Click **Save**
- A file named `CNAME` will be created in your repo — leave it alone

### 2. At your domain registrar (GoDaddy, Namecheap, Google Domains, etc.)

Add these **DNS records**:

**For the apex domain (`warewolfmanagement.com`)** — add 4 A records pointing to GitHub's IPs:

| Type | Name | Value |
|---|---|---|
| A | @ | `185.199.108.153` |
| A | @ | `185.199.109.153` |
| A | @ | `185.199.110.153` |
| A | @ | `185.199.111.153` |

**For the `www` subdomain** — add a CNAME record:

| Type | Name | Value |
|---|---|---|
| CNAME | www | `<your-username>.github.io` |

DNS propagation takes 5 minutes to a few hours. Once done, GitHub Pages will automatically provision an SSL certificate.

### 3. Enable HTTPS

Back in **Settings → Pages**, once the DNS check passes, tick **"Enforce HTTPS"**.

---

## 🧪 Testing locally before deploying

The hero video won't autoplay if you open `index.html` directly with `file://` — browsers block autoplay on local files. To test properly:

```bash
# Python 3 (comes pre-installed on macOS and most Linux)
cd warewolf-site
python3 -m http.server 8000
```

Then open http://localhost:8000 in your browser. Everything — including the hero video, fonts, and Google Maps iframe — should work.

**Alternatives:**
- `npx serve` (if you have Node.js)
- VS Code's "Live Server" extension
- Just push to GitHub and test on Pages

---

## 🎨 Editing content

Every bit of content lives in `index.html`. Common edits:

| What you want to change | Find this in `index.html` |
|---|---|
| Hero headline / tagline | Inside `<section id="home">` |
| About copy | Inside `<section id="about">` |
| 6 service cards (title + description + background image URL) | Inside `<section id="services">` — look for `class="scard reveal"` |
| 5 testimonials (name + role + photo) | Inside `<section id="testimonials">` — look for `class="tcard reveal"` |
| 10 reviews | Inside `<section id="reviews">` — look for `<!-- CARD 1 -->`, `<!-- CARD 2 -->`, etc. |
| Office address / hours / map | Inside `<section id="location">` |
| Email address | Search for `admin@warewolfmanagement.com` |
| Job listings | Inside `<section id="careers">` |
| Footer info | Inside `<footer>` |

**Images** — most section backgrounds use Unsplash URLs. To swap any image, replace the `url('https://images.unsplash.com/...')` with your own URL or local path.

**Colors** — the entire gold/black palette is defined once at the top of the `<style>` block:

```css
:root {
  --gold: #D79005;
  --gold2: #F2B006;
  --black: #080808;
  --black2: #0f0f0f;
  --black3: #161616;
  --white: #f5f0e8;
}
```

---

## 📐 What's on the site

1. **Hero** — fullscreen video background, main tagline, primary CTAs
2. **About** — company intro, founding info, address
3. **Testimonials** — 5 photo-testimonial cards from clients
4. **Services** — 6 service cards with background images:
   - Property Lead Generation
   - Listing Marketing
   - Acquisition Consulting
   - Investor Matching
   - Property Management
   - Real Estate Media & Marketing
5. **Why Choose Us** — 5 differentiators + pull quote + badges, over a fixed Chicago skyline background
6. **Vision & Mission** — two-card layout with atmospheric imagery
7. **Team** — team photo + "Join Our Team" CTA
8. **Reviews** — 10-card carousel over a dark atmospheric background
9. **Careers** — open positions
10. **Contact** — form + contact details
11. **Location** — office info + embedded Google Map
12. **Footer** — logo, nav, legal

---

## ❓ Questions

If the site isn't rendering correctly after deployment, 99% of the time it's one of:

1. **Hero video not playing** — check that `hero-bg.mp4` actually uploaded (large files sometimes silently fail in drag-and-drop on slow connections)
2. **Blank page on first visit** — GitHub Pages takes up to 10 minutes after the first push to go live
3. **Fonts look wrong** — the site uses Google Fonts (Barlow, Barlow Condensed, Cormorant Garamond) which load from `fonts.googleapis.com`; if a corporate firewall blocks that, fonts will fall back to system defaults (still readable, just less polished)
4. **Map not loading** — the embedded Google Map requires a working internet connection; it won't render offline

For anything else, check GitHub Pages logs under **Actions** tab in your repo.
