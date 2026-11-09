# samsamwanwan.com: project handoff (read this first)

Owner: Samuel Wan (Singapore, Mac user, **beginner coder**; a more experienced friend helps him). Explain things in plain language, give numbered steps, never assume terminal knowledge. Ask before committing/pushing.

## 1. What this is
Casual photo/video portfolio for occasional paid enquiries (events, personal/couples, product). Static single-page site: plain HTML/CSS/JS in one `index.html`. **No framework, no build step.** Live at https://samsamwanwan.com.

Also on the same site: a "Need a website / portfolio?" section advertising Samuel's web-dev work ($500 websites, $300 portfolios).

## 2. Hosting and deploy (GitHub Pages)
- Repo: `samuelwan04-rgb/samsamwanwan`, branch `main`, folder `/ (root)`. Pages source = "Deploy from a branch". Public repo (required for free Pages).
- Every push to `main` redeploys in ~1-2 minutes. No build, no plan to pay for. Soft limit 10 builds/hour.
- Domain registered at **Cloudflare**; DNS (all **DNS only / grey cloud**, never proxied or GitHub can't issue HTTPS):
  - 4x `A` @ -> 185.199.108.153, .109.153, .110.153, .111.153
  - `CNAME` www -> samuelwan04-rgb.github.io
- Enforce HTTPS is ON. `CNAME` file (contains `samsamwanwan.com`) must stay in repo root. `.nojekyll` must stay (stops Jekyll processing). `404.html` is the custom 404.
- Limits: published site <= 1GB (currently ~200MB), no single file > 100MB (biggest video is ~27MB), ~100GB/month soft bandwidth. GitHub terms: Pages is not for e-commerce / online businesses taking payments. A portfolio with rates and mailto/WhatsApp links is fine. Keep client shop sites off it.
- Old host: Netlify (`samuelwan.netlify.app`), kept only as a short-term backup. Safe to delete once happy.
- Samuel pushes with **GitHub Desktop** (large files; browser upload caps at 25MB/file). Optional domain verification TXT in GitHub account settings was offered but not done.

## 3. Files
```
index.html            whole site (HTML + <style> + <script>)
404.html, CNAME, .nojekyll, robots.txt, sitemap.xml
favicon.svg, favicon-32.png, apple-touch-icon.png   (italic gold Fraunces "S" on #0b0a09)
img/*.jpg             39 photos, 1800px wide full size
img/t/*.jpg           800px wide thumbnails (quality 78), same filenames
videos/               main videos, posters (*.jpg), bg-*.mp4 desktop loops, m-bg-*.mp4 phone loops
```
Main videos: `brand-new-day-coretone.mp4`, `firstcook.mp4`, `koreavlog.mp4`, `rinis-21.mp4` (27MB, 1080p, copied unmodified from Samuel's export). Background loops: `bg-coretone`, `bg-rinis`, `bg-firstcook`, `bg-korea` (+ `m-bg-*` smaller 854px versions for phones).

## 4. Design system
- Tokens in `:root`: `--bg:#0b0a09 --ink:#f1ebe2 --mute:#b3aa9b --amber:#e0a458 --soft --soft2 --panel`, fonts Fraunces (serif, display) + Inter (sans) via Google Fonts. Dark only (`color-scheme:dark`).
- Mood: dark, cinematic, warm amber accent, serif headings, uppercase tracked kickers. Casual friendly copy ("Wanna chat? :)", "HMU!"). Don't over-polish the voice.
- Layout: doors hero -> Events (01) -> Personal & Couples (02) -> Product (03) -> B-roll (04) -> Video (#reel) -> Rates -> Websites -> Contact (#contact). Nav shows only Rates/Websites/Enquire on phones (<=800px).

## 5. Behaviours (don't break these)
- **Fixed looping video background** (`#bgwrap`, `<video id="bgA">` + `bgB`). Desktop: playlist with crossfade, B preloads 4s after A starts. Phones (<=800px): one video at a time using `m-bg-*` files (two big videos at once made phones stall). `nudge()` restarts a paused bg video. Respects Data Saver (`LIST=[]`) and `prefers-reduced-motion`. A **Pause/Play button** (bottom-right) is required for accessibility (WCAG 2.2.2).
- **Doors hero**: three full-height door images. Mouse: hover expands, click goes to section. Touch: first tap expands (`.open`), second tap scrolls to the section; tapping elsewhere closes. Same on iPhone and Android.
- **Parallax**: hero layers, photos shift inside frames, rows rise in (IntersectionObserver). JS batches reads then writes. Disabled by reduced-motion.
- **Photo grid**: `.rows > .row > button.ph` with `--a` (aspect ratio) for justified rows on desktop; on phones `.row{display:contents}` makes a 3-column grid of square tiles (Instagram style). Tapping opens the **swipe viewer** (`#vw`): scroll-snap track, dots, arrows on desktop, Esc/arrow keys, background `inert` + focus trap while open.
- **Images**: each grid `<img loading="lazy" src="img/X.jpg" srcset="img/t/X.jpg 800w, img/X.jpg {W}w" sizes="(max-width:800px) 34vw, (max-width:1240px) 45vw, 560px" width height alt>`. The viewer reads the plain `src` (full size).
- **Videos** (`#reel`): `preload="none"` with poster frames; captions: "If Peter Parker needed to fix his guitar..." (coretone), "Our first cook in my renovated kitchen!", "Rinis's 21st", "KOREA 2026 VLOGGG".
- SEO/sharing: absolute `og:image` (`https://samsamwanwan.com/img/jwe03182.jpg`), twitter card, canonical, JSON-LD, theme-color.

## 6. How to add things
**New photo:** make `img/NAME.jpg` (1800px wide, sRGB, ~quality 85) and `img/t/NAME.jpg` (800px wide, quality 78). Add a `button.ph` with the right `--a` (width/height), `width`/`height` attrs and `srcset`. Put it in the right section row. Add a real descriptive `alt`.
**New video:** 1080p H.264, `-preset slow -crf 21-23` with `-maxrate`/`-bufsize` caps (uncapped crf explodes file size), AAC audio, `-movflags +faststart`, plus a poster `.jpg`. Keep each file well under 100MB. Background loops: no audio, crf 27-30, maxrate 1.3-2M; phone loops 854px wide.
**Tools** (if needed): ffmpeg, Playwright for headless screenshots. Test at 390px (phone) and 1440px (desktop); check no sideways overflow and no console errors.

## 7. Content facts
- Tagline: "Your friendly neighbourhood photo guy. I do events, products and photoshoots. Wanna chat? :)"
- Rates: Couples/Personal photoshoot 2h **$100**. Concert 2 sets x 45 min **$200**. Event 4h **$300** (edits within 18h). Product and Video: "Let's chat!". Websites **from $500**, portfolios **from $300** (+ about $25 per 6 months for small later updates), extra features cost extra. Custom domain is a yearly fee: client buys/renews themselves, or Samuel handles it for a monthly / 3-monthly / 6-monthly retainer.
- Contact: samuelwan04@gmail.com, WhatsApp +65 9625 8955 (wa.me/6596258955), Instagram @samsamwanwan. Footer "(c) YEAR SAMUEL. / SINGAPORE".
- Client/own sites shown in the Websites section (all still on Netlify/GitHub URLs, update the links when each moves): Luna Voce (lunavoce.netlify.app), SmartHouzz (smarthouzz.netlify.app), Dean's Art & Animation (deansartanimation.netlify.app), Midiator (samuelwan04-rgb.github.io/midiator), Core Tone Singapore concept (coretonesg.netlify.app), and this site.
- Web business model for client sites: A = client owns everything; **B = client owns domain, Samuel hosts (recommended)**; C = Samuel manages everything. A signable "Website Build Agreement" Claude Doc exists: https://claude.ai/artifact/UnebnbEECJ2iSrGAtmfBKs

## 8. Gear context (for copy/blog ideas, not needed to edit)
Sony a7III, S-Log3 video; Tamron 28-75 f/2.8 G2 (67mm), Sigma 56 f/1.4, Sony FE 16-35 f/4 (72mm), FE 55 f/1.8. Has a free Hoya 67mm ND8.

## 9. Audit status
An Impeccable-style audit scored the site 14/20, then fixes were applied (OG image, pause button, nav landmark + skip link, tap targets, thumbnails, scroll perf, viewer focus trap, mobile tagline, SEO files). The Impeccable skill is third-party and **not installed**; its launcher binary was blocked by the safety check, so the audit was done by hand from its checklist. Not yet tested on a real phone (tap/two-tap doors, bg video loop, parallax feel).

## 10. Open to-dos / ideas (none decided unless stated)
1. **Photo alt text** is generic ("Events photo 1"). Write real descriptions (ask Samuel or look at the images).
2. Test on a real iPhone/Android: bg video keeps looping, doors two-tap, grid and viewer.
3. "Recent events" section with unlisted per-event galleries and downloads (Drive-hosted), plus an option to send photos directly. Open question: are most events private (army/church)? Do not publish clients' photos without consent. Do not host private client files in this public repo.
4. Drive-driven gallery so Samuel can drop photos in a Drive folder without redeploying (needs a Drive folder and API key; not started).
5. Add About photo + short bio, testimonials, "what's included" lines in Rates, preview thumbnails for the client sites (needs screenshots).
6. Move the other sites (Luna Voce, SmartHouzz, etc.) to GitHub Pages one repo each, then update links in the Websites section.
7. Agreement doc has blank retainer amounts and prices to fill.
8. Optional: verify the domain in GitHub account settings (TXT in Cloudflare).

## 11. Working rules for Claude Code
- Edit files directly in the repo; keep it a static site, no new frameworks.
- Before changing visuals, screenshot at 390px and 1440px; after, re-check overflow and console errors.
- Show Samuel what changed in plain language, then ask before `git commit` / `git push`. Never force-push. Never delete `CNAME`, `.nojekyll`, `404.html`.
- Keep photos/videos light (see sizes above). Warn before adding any single file over 50MB.
- Don't touch DNS or Cloudflare; give Samuel steps instead.
