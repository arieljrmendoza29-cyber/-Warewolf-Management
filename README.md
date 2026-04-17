# WareWolf Management

Single-page website for WareWolf Management — Chicago's real estate management firm. Fully self-contained, no build step, runs on any static host.

## Contents

| File | Purpose | Size |
|---|---|---|
| `index.html` | The entire site — HTML, CSS, and JavaScript inlined | ~55 KB |
| `logo.jpg` | Logo used in nav and footer | ~77 KB |
| `hero-bg.mp4` | Background video for the hero section (landscape) | ~0.85 MB |
| `tour-1.mp4` … `tour-5.mp4` | Property tour videos for the "Properties in Motion" section (portrait) | ~9 MB total |

Total size: **~10 MB**. All videos are H.264, muted (audio stripped), and encoded with `+faststart` so they stream progressively while downloading.

## Local preview

Because the site loads videos as relative files, opening `index.html` with a `file://` URL may be blocked by some browsers' autoplay rules. Running a simple local server avoids this:

```bash
# Python 3 (no install needed on most systems)
python3 -m http.server 8000

# OR — Node (if you have it)
npx serve .
```

Then open <http://localhost:8000>.

## Deploying to GitHub Pages

### One-time setup

1. Create a new repository on GitHub (e.g. `warewolf-site`).
2. From the site folder, initialize git and push:

```bash
git init
git add .
git commit -m "Initial commit: WareWolf Management site"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/warewolf-site.git
git push -u origin main
```

3. On GitHub, go to **Settings → Pages**.
4. Under **Source**, pick **Deploy from a branch**, choose `main` branch and `/ (root)` folder, and click **Save**.
5. Wait 1–2 minutes. Your site will be live at `https://YOUR-USERNAME.github.io/warewolf-site/`.

### Custom domain

1. In **Settings → Pages**, add your domain (e.g. `warewolfmanagement.com`) under **Custom domain**.
2. At your DNS provider, add these records:
   - **A records** for `@` pointing to GitHub's IPs: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - **CNAME** for `www` pointing to `YOUR-USERNAME.github.io`
3. Back on GitHub, enable **Enforce HTTPS** once DNS propagates (can take a few hours).

### Updating the site

```bash
# Make your edits to index.html, then:
git add .
git commit -m "Describe what you changed"
git push
```

GitHub Pages rebuilds automatically within a minute.

## Deploying elsewhere

Any static-file host works — the whole site is just files:

- **Netlify**: drag-and-drop the folder onto <https://app.netlify.com/drop>
- **Cloudflare Pages**: connect the GitHub repo, set build command to `(none)` and publish directory to `/`
- **Vercel**: connect the GitHub repo, framework preset "Other"
- **AWS S3 / DigitalOcean Spaces / any static bucket**: upload the folder contents as-is

## Notes on the videos

The six `.mp4` files are ~1–3 MB each after compression. If file size ever becomes an issue for your Git history, consider [Git LFS](https://git-lfs.com/) — but for files this size, plain Git is fine and GitHub has no problem with it (their limits are 100 MB per file and 1 GB per repo soft cap).

If you want to swap a video out:

1. Replace the relevant `.mp4` file (keeping the same filename).
2. For best performance, compress with ffmpeg:
   ```bash
   ffmpeg -i input.mp4 -vf "scale=-2:720" -c:v libx264 -profile:v main \
          -preset slow -crf 30 -an -movflags +faststart output.mp4
   ```
3. Commit and push.

## Editing captions, text, or the carousel

Open `index.html` and search for these anchor points:

- **Carousel items** — search for `<!-- MARQUEE -->` (around line 310)
- **Hero headline + tagline** — search for `class="hero-h1"`
- **Gallery section captions** — search for `<!-- IN MOTION / GALLERY -->`
- **Navigation links** — search for `<nav id="mainNav">`
- **Contact info + email** — search for `id="contact"` and `203 N LaSalle`
- **Color palette** — search for `--gold:` (all site colors are CSS variables at the top of `<style>`)

## Browser support

Targeted at modern browsers (Chrome, Firefox, Safari, Edge — latest two versions). Videos use H.264 Main profile which is universally supported. The site gracefully degrades: if a video fails to load, a dark gradient shows in its place.

Responsive breakpoints:
- Tablet / medium screens: ≤ 960px
- Phones: ≤ 640px

Users with "reduce motion" enabled in their OS settings will not see autoplaying videos — they're paused automatically.
