# Mycel

Browser-based node-graph generative art studio. Static site, no build, p5.js via CDN.

## Run locally

```bash
python3 -m http.server 5174
# open http://localhost:5174
```

## Deploy

### Netlify (drag & drop)
Drop the project folder onto https://app.netlify.com/drop. Done. `netlify.toml` sets the camera/mic permission policy and security headers.

### Vercel
```bash
npx vercel --prod
```
`vercel.json` provides the same headers + clean URLs.

### Cloudflare Pages
1. Push to a Git repo.
2. Cloudflare Pages → Create project → connect repo.
3. Build command: *(none)*. Build output directory: `/`.

### GitHub Pages
1. Push to a repo.
2. Settings → Pages → Source: `main` / root.
3. Live at `https://<user>.github.io/<repo>/`.

## Custom domain (DNS)

Buy a domain (Namecheap, Porkbun, Cloudflare Registrar). Suggested: `mycel.studio`, `mycel.app`, `mycel.art`.

### Netlify
1. Site → Domain management → Add custom domain → enter `mycel.studio`.
2. In your registrar's DNS panel, add:
   - **A record**: `@` → `75.2.60.5` (Netlify's apex IP)
   - **CNAME**: `www` → `<your-site>.netlify.app`
3. Or, if your registrar supports it, use Netlify DNS — set nameservers to `dns1.p01.nsone.net` etc. (Netlify will show the exact four).
4. Netlify auto-provisions a Let's Encrypt cert in ~minutes.

### Vercel
1. Project → Settings → Domains → add `mycel.studio`.
2. In registrar DNS:
   - **A record**: `@` → `76.76.21.21`
   - **CNAME**: `www` → `cname.vercel-dns.com`
3. Cert auto-provisions.

### Cloudflare Pages
1. Pages project → Custom domains → add `mycel.studio`.
2. If domain is already on Cloudflare DNS, it's instant. Otherwise add the CNAME they show.

### General DNS notes
- Apex (`@`) records must be **A** (or **ALIAS**/**ANAME** if your provider supports it). CNAME on apex is non-standard.
- TTL: leave at default (300–3600s).
- Propagation usually ≤ 15 min; can take up to 48h worst case.
- Verify with `dig mycel.studio +short` or https://dnschecker.org.

## OG image

Add `og.png` (1200×630) at the root for nice link previews on Twitter/Slack/iMessage. Easiest: open the studio, load `Liquid Mandala`, set resolution to `1080²`, screenshot a 1200×630 crop, save as `og.png`.

## Files

| File | Purpose |
|---|---|
| `index.html` | Studio (the app) |
| `gallery.html` | Public gallery of remixable graphs |
| `flowfield.html` | Standalone flow-field demo |
| `netlify.toml` / `vercel.json` | Hosting config |
| `sitemap.xml` / `robots.txt` | SEO |
| `README.md` | This file |

## Development notes

- All node logic lives in `NODE_DEFS` inside `index.html`. To add a node, define `inputs`, `params`, `proc(n, ins, t, g)`. Optional `post()` runs after the main pass (used by `Feedback`).
- Stateful nodes (FlowField, AntTrail, Feedback, Webcam, SquareTracking) keep their state on `n.state` and key it to a signature so seed/size changes reset cleanly.
- Shared graphs are gzipped + base64-url-safe-encoded into `location.hash` (`#g.…`). Falls back to plain JSON (`#j.…`) if `CompressionStream` is unavailable.
- Embed mode: append `?embed=1` to any URL — hides all chrome.

## Distribution playbook

See `STRATEGY.md` if you split out a separate doc; otherwise the priority list:

1. **Share each creation** to Twitter/Threads with the remix link in the caption.
2. **Daily on Instagram/X** — one piece a day, watermarked subtly.
3. **Show HN** when domain + gallery are live.
4. **r/generative**, **r/proceduralgeneration** — show, don't pitch.
5. **Audio/webcam reactivity** is your viral hook — every "yourself in 30 seconds" clip drives signups.
