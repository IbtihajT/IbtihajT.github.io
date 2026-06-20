# Visual Redesign — Session Handoff (started 2026-06-20)

> **Purpose:** Pick up the portfolio visual redesign cold on another machine without re-discussing. Read this, then `docs/mockups/NOTES.md`, then open the mockups.

## TL;DR — where we are

The current live site is well-built but its aesthetic (`#06060f` near-black + violet→cyan glow + glassmorphism bento + Outfit/Inter/JetBrains-Mono + numbered `01/02` labels + animated gradient orbs) is the single most templated "AI-generated engineer portfolio" look right now. The redesign goal: a **distinctive identity grounded in Ibtihaj's actual domain — turning signal/data into meaning** (vocal-emotion audio ML, malware-as-image CNN, latent-space viz). That domain hands us a visual language (spectrograms, waveforms, latent scatter) no template has, which doubles as proof of competence.

We explored two directions as interactive mockups. **Owner chose Direction A — "Spectral Lab" (light scientific workbench).** We have NOT touched the real Astro site in `src/` yet — exploration only.

## Status

- [x] Research (what makes ML portfolios memorable; cliché anti-patterns; credibility techniques)
- [x] Two interactive mockups built in `docs/mockups/`
- [x] **Owner picked Direction A — Spectral Lab**
- [ ] Awaiting owner's detailed reaction to A (gut feel / hero spectrogram / anything that bugged them) — see "Open questions" below
- [ ] Refine mockup A (see "Refinement instincts")
- [ ] Port the chosen, refined design into the real Astro `src/` components
- [ ] Remove `docs/mockups/` (and consider this doc) once the redesign ships

## The mockups

Self-contained HTML, no build step. Serve locally to resolve fonts/images/canvas:
```
python3 -m http.server 4321   # from repo root
# then open http://localhost:4321/docs/mockups/index.html
```
(Or double-click the files — `file://` works too.)

- `docs/mockups/direction-a-spectral-lab.html` ← **CHOSEN**
- `docs/mockups/direction-b-oscilloscope.html` (dark phosphor instrument — rejected for now, kept for reference)
- `docs/mockups/index.html` — landing page linking both
- `docs/mockups/NOTES.md` — **exact token system** (hex, fonts, signature element, layout language) for both directions

### Direction A — "Spectral Lab" token system (summary; full detail in NOTES.md)
- **Palette:** paper `#F2F0E9` (cool lab off-white), ink `#1A1A1A`, muted `#5A5A52`, hairline `rgba(26,26,26,0.16)`. Only two chromatic accents: oscilloscope amber `#E8A33D` (data-trace, sparing) + cool data-blue `#2E6F95` (links, stat numbers).
- **Type:** display **Spectral** (serif, literally named Spectral) · body **IBM Plex Sans** · data/labels **IBM Plex Mono**.
- **Signature:** generated **spectrogram + waveform** hero on `<canvas>`; **projects rendered as scientific figures** each with a generated domain artifact (waveform / CNN feature-map / latent scatter), figure caption, mono tag chips, and an honest editable `⟨add⟩` metric line.
- **Structure:** lab-notebook layout, hairline rules, mono micro-labels carrying *real* metadata (NOT `01/02` numbering).

## Refinement instincts (agreed starting point for the next pass on A)

1. **Trim the lab-costume metadata.** Keep meaningful labels (`FIG. 1`, spectrogram axes `FREQ ↑ · TIME →`, `INPUT → / OUTPUT →`). Cut decorative theater (`PLATE A · operator`, `REC … 44.1 kHz`, `SUBJECT · M.I.T`) so the real labels carry weight.
2. **Make the grid mean something.** Confine the graph grid to the figure/canvas panels (where a grid is data); let the paper breathe elsewhere instead of a full-page 32px grid.
3. **Braver hero.** Name + tagline currently stack conventionally above the spectrogram. Explore letting the signal and the name share space more boldly — the signal *is* the hero.

## Open questions for the owner (capture answers here next session)

- Gut reaction to the light scientific feel — exciting, or too cold/academic for clients & recruiters?
- Hero spectrogram — keep as-is, or try variations (cleaner single waveform / more literal STFT heatmap)?
- Anything that immediately bugged them?

## Research findings (condensed — full brief was generated this session)

**Principles that make ML portfolios memorable:** substance-over-style restraint reads as confidence (cf. karpathy.ai); credibility from verifiable artifacts not adjectives; one signature mechanic executed well; the Distill.pub "scientific communication" lineage signals taste; narrative beats inventory; honest evaluation (precision/recall/F1, not just "99% accuracy") is the rarest high-signal element; dual-depth structure (scannable in 40s, deep dive available).

**Cliché anti-patterns to avoid:** near-black + single bright accent + glassmorphism; animated gradient orbs / nebula glow; the Outfit/Inter/JetBrains-Mono trio; numbered `01/02/03` labels; "trained a CNN to 99%" with no eval honesty; identical glowing card grids; cold 2010s grays. *(The current site uses several of these — that's what we're leaving behind.)*

**Credibility techniques realistic for a static Astro site:**
- *Easy:* real metrics tables/figures (confusion matrix, F1); inline architecture/pipeline diagrams; syntax-highlighted code; dated timeline; repo links.
- *Easy & strongest differentiator:* pre-rendered real domain artifacts as imagery — actual spectrograms, waveforms, t-SNE/UMAP latent scatter, CNN feature maps (librosa/matplotlib exports) from his real projects.
- *Medium:* embed a live demo via Hugging Face Spaces / Gradio iframe (free, always-online); 1–2 min screen recording per project.
- *Ambitious:* client-side interactive viz (D3 / ONNX-web) — in-page latent explorer or live spectrogram of uploaded audio.
- *Project presentation verdict:* card grid as entry points → **case-study deep-dives** for the 3 real projects (problem → solution → honest metrics) convert best for ML roles.

**The other directions considered (for reference if A is ever abandoned):** B "Oscilloscope" (dark phosphor instrument — built, see mockup); "Distill Notebook" (research-paper white + serif + ML-blue); "Editorial Slate" (warm slate + oxblood + Fraunces).

## Next steps

1. Get owner's reaction (Open questions above), record answers here.
2. Refine mockup A per the three instincts + owner feedback.
3. Once A feels right, port into Astro: rework `src/styles/global.css` `@theme` tokens (paper/ink/amber/blue, Spectral + IBM Plex), then component-by-component (`Hero`, `About`, `Projects`, `Services`, `Stack`, `Contact`, `Nav`, `Footer`, `SectionLabel`). The spectrogram/figure canvases become small client scripts or pre-rendered SVGs.
4. Reconcile with `docs/future-features.md` D1–D5 (hero typography, role rotation, hover polish, page-per-scroll, section vertical fill) — several become moot or change under the new design.
5. Delete `docs/mockups/` once shipped.
