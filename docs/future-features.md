# Future Features Backlog

> **Purpose:** A living list of features intentionally deferred from the initial Astro rebuild. Each entry has enough context for any future session (human or Claude) to pick up cold without re-discussing.

> **Status legend:** `IDEA` = needs design / discussion · `READY` = scope is clear, just implement · `BLOCKED` = waiting on external input

---

## 1. Blog system — `READY`

**Why deferred:** No posts written yet. Building empty blog infra now would be premature; design will be clearer once there's real content to style.

**Scope when picked up:**
- `src/content/blog/*.md` content collection with Zod schema (`title`, `date`, `description`, `tags[]`, `draft`).
- `src/pages/blog/index.astro` — listing page with tag filter (reuse the same React island as the project filter, or vanilla).
- `src/pages/blog/[slug].astro` — individual post via `getStaticPaths`.
- `src/layouts/BlogPost.astro` — typography wrapper using `@tailwindcss/typography` `prose` classes.
- Syntax highlighting via Astro's built-in `expressive-code` integration (frame, copy button, line highlights out of the box).
- RSS feed via `@astrojs/rss` + sitemap via `@astrojs/sitemap`.
- Add `/blog` link to nav.

**Open decisions when starting:**
- Tag taxonomy — share with projects, or independent?
- Reading-time display? (use `reading-time` npm package)
- "Last updated" vs "Published" — show both or just one?

---

## 2. Contact form — `BLOCKED` (need to pick a service)

**Why deferred:** Owner wants to decide on a service later. Current Hero/Contact section can use a `mailto:` placeholder.

**Candidate services (no signup needed for the form action itself in any of these):**

| Service | Free tier | Notes |
|---|---|---|
| **Formspree** | 50 submissions/mo | Industry standard. Spam protection (reCAPTCHA optional). |
| **Web3Forms** | Unlimited | Newer. No rate limit advertised. Simple POST to their endpoint. |
| **FormSubmit** | Unlimited | Zero-config, posts to your email. Lower polish than Formspree. |
| **Getform** | 50 submissions/mo | Comparable to Formspree. Better UI for inbox. |

**Implementation pattern (same for all):**
```html
<form action="https://<service>/<id>" method="POST">
  <input name="name" required />
  <input name="email" type="email" required />
  <textarea name="message" required></textarea>
  <button type="submit">Send</button>
</form>
```
Add honeypot field for spam: `<input name="_gotcha" style="display:none" />`. Most services support this.

**Open decisions when starting:**
- Pick service.
- Add client-side validation? (HTML5 attributes are probably enough.)
- Success/error state — inline message or redirect to `/thank-you` page?

---

## 3. Downloadable CV PDF link — `READY` (but see #4 for the better long-term version)

**Why deferred:** Tied to #4 — owner wants the PDF to auto-update from a LaTeX source repo rather than be manually re-uploaded.

**Quick-and-dirty version (until #4 is built):**
- Drop the current PDF at `public/cv.pdf`.
- Add a "Download CV" button in Hero or About section pointing to `/cv.pdf`.
- Update manually when CV changes.

**Skip this and go straight to #4** if owner is ready to set up the cross-repo pipeline.

---

## 4. Auto-updating CV via LaTeX source repo — `IDEA` (owner's idea, worth designing)

**Owner's idea (verbatim):** "I maintain and update my CV in LaTeX that is available on Github as a repository. I wonder if there would be a way to recompile the CV for everytime someone downloads my CV from my Portfolio Website. This ensures that I don't have to keep uploading my CV everytime I make changes to it."

**Goal:** Single source of truth (the LaTeX repo). Portfolio always serves the latest compiled PDF without manual upload.

**Options, ranked by simplicity:**

### Option A — GitHub Release artifact (recommended)
1. In the LaTeX CV repo, add a GitHub Action that runs on every push to `main`:
   - Compile LaTeX → PDF (use `xu-cheng/latex-action` — well-maintained TeXLive container).
   - Create/update a release tagged `latest`, attaching the PDF.
2. In the portfolio, the "Download CV" button links to the stable GitHub release URL:
   `https://github.com/IbtihajT/<cv-repo>/releases/latest/download/cv.pdf`
3. Done. Every CV change auto-publishes; the portfolio link never breaks.

**Pros:** Zero coupling between repos. No portfolio rebuild needed. URL stays stable.
**Cons:** Visitor's download is served from github.com, not the portfolio domain (cosmetic).

### Option B — Cross-repo deploy
1. LaTeX CV repo's Action compiles PDF and uses a personal access token to commit `cv.pdf` into the portfolio repo's `public/` folder.
2. Portfolio's deploy Action runs, redeploying with the new PDF.

**Pros:** PDF served from the portfolio domain (`ibtihajt.github.io/cv.pdf`).
**Cons:** Requires PAT in CV repo secrets. Triggers a portfolio rebuild on every CV edit (slightly wasteful but harmless).

### Option C — On-demand compile (overkill)
Serverless function (Vercel/Netlify) that runs `pdflatex` on request. Cold-start latency, more infrastructure, no real win over A or B. Skip.

**Recommended path:** Option A first (15 min of setup, zero complexity). Migrate to Option B if the github.com download URL ever bothers the owner.

**Need from owner when starting:**
- Name / URL of the LaTeX CV GitHub repo.
- Confirmation that the Action can write to releases (default is yes).

---

## 5. Privacy-friendly analytics (GoatCounter) — `READY`

**Why GoatCounter:** Free for non-commercial use, ~2KB JS, no cookies, no consent banner needed under GDPR, open source. Shows pageviews, referrers, browser/OS, screen size. No per-user tracking.

**Scope when picked up:**
- Sign up at `goatcounter.com`, get a site code.
- Add the snippet to `src/layouts/Base.astro` inside `<head>`:
  ```html
  <script
    data-goatcounter="https://<your-code>.goatcounter.com/count"
    async src="//gc.zgo.at/count.js"></script>
  ```
- Add `<noscript>` fallback if desired.

**Alternatives if GoatCounter doesn't appeal:**
- **Plausible** (self-host or paid SaaS, ~$9/mo).
- **Umami** (self-host, free, needs a database).
- **Cloudflare Web Analytics** (free, even more lightweight, but requires the site to be behind Cloudflare).

---

## 6. Giscus comments on blog posts — `READY` (depends on #1)

**Why Giscus:** Free, comments stored as GitHub Discussions in your repo, visitors sign in with GitHub. No database, no moderation tooling needed beyond GitHub's, no ads.

**Scope when picked up:**
1. Enable Discussions on the portfolio repo (or a dedicated comments repo).
2. Install the Giscus GitHub App on that repo.
3. Configure at `giscus.app` — get a `<script>` snippet with `data-repo`, `data-repo-id`, `data-category`, `data-category-id`.
4. Add the snippet to `BlogPost.astro` layout (or a dedicated `<Comments />` Astro component) at the bottom of each post.
5. Theme: pass `data-theme` matching the site's dark theme (`dark_dimmed` or `noborder_dark` work well with violet/cyan).

**Depends on:** Feature #1 (blog) must exist first.

---

## 7. Dark / light theme toggle — `IDEA` (needs design work)

**Why deferred:** Current portfolio.css design tokens are dark-only. A proper light theme needs a designed palette — not just inverted colours — because the violet/cyan glow effects, glassmorphism, and bento contrast all rely on dark backgrounds.

**Scope when picked up:**
1. Design a light palette that preserves brand identity (probably warm off-white bg + deeper violet/cyan accents, plus rethought glass/glow treatment).
2. Move tokens into `@theme` blocks for both modes — Tailwind v4 supports this via `@variant dark`.
3. Add a toggle component (sun/moon icon) in the Nav. Use `astro-icon` for the icons.
4. Persist preference in `localStorage`, respect `prefers-color-scheme` on first visit.
5. Use Astro's `<ClientRouter />`-compatible script (theme set BEFORE first paint to avoid flash).

**Open decisions when starting:**
- System-default detection or always-dark-by-default?
- Per-page override?
- Animate the toggle transition?

---

## Index / quick-start map for future sessions

When picking any of these up:
1. Read this file first.
2. Read `CLAUDE.md` for the current project context (may have moved on since this file was written).
3. Confirm with the owner the feature is still wanted before starting — priorities shift.
4. Cross off completed items by deleting their section (or moving to `docs/completed-features.md` if a record is wanted).

---

# Design polish backlog
> Captured during the Astro migration. Owner's priority is "ship the rebuild first" — these are deferred deliberately. Revisit after the live site is up.

## D1. Hero typography iteration — `IDEA`
**Context:** First-pass sizing for the giant name was set to `clamp(48px, 8vw, 120px)` (desktop) when we changed the displayed name from "Ibtihaj / Tahir" to "Muhammad Ibtihaj / Tahir" on 2026-05-11. The legacy was tuned for the shorter "Ibtihaj" line at `clamp(70px, 12vw, 170px)`. The new sizing is functional but may not feel as impactful — needs a design review across viewports (320px, 768px, 1024px, 1440px, 1920px).

**Things to explore:**
- Try `clamp()` lower bounds for ultra-narrow phones so "Muhammad Ibtihaj" doesn't wrap or look cramped
- Alternative line splits: "Muhammad / Ibtihaj Tahir", "Muhammad / Ibtihaj / Tahir" (3 lines), or single line "Muhammad Ibtihaj Tahir"
- Consider a smaller "Muhammad" above the main "Ibtihaj Tahir" lockup (kicker pattern)
- Adjust `letter-spacing` and `line-height` together — current `-0.045em` / `0.86` was tuned for the larger sizes
- Optical kerning between "I" and "b" in "Ibtihaj" (Outfit's default may be loose)

## D2. Role rotation cadence / transition cue — `IDEA`
**Context:** The hero's "— Data Scientist / AI Engineer / ML Engineer" role rotator uses a CSS-only `content:` animation on a 9s loop (3s per role, hard cut). Owner felt either the cadence is too slow, or there's no visual cue that the text is about to change.

**Things to explore:**
- Shorter cycle: 6s loop (2s per role) — see if it feels less stale
- Add a cursor / caret blink before the cut (`::before { content: "▎"; animation: blink ...}`) to signal "typing-style" intent
- Subtle fade-out/fade-in between roles instead of hard cut. Since `content:` is discrete, this would require either:
  - JS-based rotation (small island, ~20 lines) so we can fade the wrapper element
  - Two stacked spans with staggered opacity animations, each containing all three role strings on different keyframe offsets
- Underline-grow animation under the role text as a "transition is coming" tell
- A progress bar / dot indicator showing position in the cycle

## D3. Cross-component hover polish pass — `IDEA`
**Context:** After porting all sections, do a full hover review against the no-lift / centered-glow rule (see `memory/feedback_hover_design.md`). Some elements may need more glow spread, different brightness deltas, or animation timing tweaks to feel cohesive.

## D4. Page-per-scroll navigation — `IDEA`
**Owner's note (2026-05-11):** "I think the scrolling mechanism should be per page per scroll. We can explore this later."

**Interpretation:** Each scroll-wheel tick / swipe should advance to the next section rather than scrolling continuously through pixels — Hero → About → Projects → Services → Stack → Contact. Section IDs already exist for anchors.

**Things to explore:**
- **CSS-only:** `scroll-snap-type: y mandatory` on `<html>` or `<main>` + `scroll-snap-align: start` on each section. Pros: zero JS, accessibility-friendly, respects scroll-behavior. Cons: feels closer to "sticky stop" than the dramatic "advance one screen per swipe" of Fullpage.js.
- **JS library (`fullpage.js` / `@fullpage/react-fullpage`):** True one-section-per-scroll, dot pagination, keyboard nav. Pros: polished feel. Cons: extra JS (~70KB), licence cost for commercial use (free for open source/personal), accessibility caveats.
- **Custom vanilla solution:** Listen for `wheel` events with a debounce/lock, animate `window.scrollTo()` to the next section's offsetTop. ~40 lines. Pros: full control, no dep. Cons: have to reimplement keyboard, touch, and accessibility manually.
- **Hybrid:** CSS scroll-snap-stop with a small JS layer that adds the "one scroll = one section" lock.

**Open decisions when starting:**
- Should it apply to every viewport size, or only desktop? Mobile users typically expect free-scroll.
- What happens on a section that's taller than the viewport (e.g. Projects bento grid on a small laptop screen)? Free-scroll within, snap between?
- Keyboard nav: ↑/↓ keys, PgUp/PgDn, and # anchor links should all work.

## D5. Section vertical fill — `IDEA`
**Owner's note (2026-05-11):** "The desktop version is not utilizing the full section lenght and the content only covers like top 50% of the About section placeholder."

**Context:** Current section padding is `110px 0` from the legacy. On wide desktops with short content (e.g., About's image + tagline + 7 tags), the section may only fill ~50% of viewport height, leaving a tall band of background below before the next section begins.

**Things to explore:**
- Add `min-height: 100vh` to sections so each fills at least one viewport (closely related to D4 page-per-scroll).
- Add `min-height: min(100vh, 900px)` to avoid huge gaps on 4K monitors.
- Use `align-items: center` on the section's inner grid so content vertically centers in the available space — currently it sits at the top because the section is just padded.
- Reconsider section padding (`110px 0`) — could be `clamp(60px, 10vh, 140px)` for proportional spacing.
- This decision will affect every section, so iterate on About first then propagate.

