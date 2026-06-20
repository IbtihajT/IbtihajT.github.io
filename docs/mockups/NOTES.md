# Portfolio Direction Mockups — Token Systems

Throwaway exploration artifacts. Open `index.html` to compare both. Real Astro site in `src/` is untouched.

Each file is self-contained: inline `<style>`, Google Fonts via `<link>`, vanilla JS (canvas/SVG) inline. No build step; works via `file://` double-click.

Assets referenced (relative to `docs/mockups/`):
- Portrait: `../../public/images/MyStack/Me.jpg`
- Stack logos: `../../public/images/MyStack/{python,bash,PostgresSQL,Pandas,PyTorch,TensorFlow,scikit-learn}.svg`

Honesty note: all hard metrics are obvious editable placeholders — `⟨add⟩` styled in the accent color. No invented accuracy/F1 numbers.

---

## Direction A — "Spectral Lab" (light scientific workbench)

`direction-a-spectral-lab.html`

### Colors (hex)
| Token | Value | Use |
|---|---|---|
| paper | `#F2F0E9` | page background (cool lab off-white) |
| graph grid | `rgba(26,26,26,0.06)` | faint 32px background grid lines |
| ink | `#1A1A1A` | primary text, primary data trace |
| muted ink | `#5A5A52` | secondary text, captions |
| oscilloscope amber | `#E8A33D` | DATA-TRACE accent only, used sparingly (high-energy spectrogram bins, secondary divider) |
| cool data accent | `#2E6F95` | links, secondary data trace, stat numbers |
| hairline | `rgba(26,26,26,0.16)` | all rules and card borders |

Only two chromatic colors: amber + cool-blue. Disciplined.

### Fonts (Google Fonts)
- Display / headings: **Spectral** — weights 500, 600 (the serif literally named Spectral; 600 + occasional 500 italic for tagline)
- Body: **IBM Plex Sans** — 400, 500
- Data / labels / captions / micro-metadata: **IBM Plex Mono** — 400, 500

### Layout language
- Lab-notebook / scientific-figure structure. Hairline rules, generous margins, 1080px max width.
- Mono micro-labels carry real metadata, NOT 01/02 numbering: `FIG. 1`, `INPUT →`, `OUTPUT →`, `REC 2026-06-20 · 44.1 kHz`, `FREQ (Hz) ↑ · TIME (s) →`, `PLATE A`, units.

### Signature element
- **Generated spectrogram + waveform** hero centerpiece on `<canvas id="spectro">`: STFT-style frequency-vs-time columns (ink, with amber for high-energy low-freq bins) over the faint grid, plus a cool-blue waveform overlay. Captioned as a figure ("Fig 1 — vocal emotion · spectral features"). Animated subtly; reduced-motion renders a single static frame.
- **Waveform SVG dividers** reuse the motif between sections (alternating ink / amber).
- **Projects as scientific FIGURES**, each a small generated canvas thumbnail:
  - Fig 2a (Singing Emotion) = layered waveform
  - Fig 2b (Malware CNN) = pixelated CNN feature-map / binary grid
  - Fig 2c (MNIST) = latent-space scatter of dots (3 clusters in ink/blue/amber)
  - Each has a figure caption, mono tag chips, and one placeholder metric line.

### Deliberately avoided
Warm cream + terracotta + high-contrast boho serif (AI cliché). The cool paper, cool-blue accent, IBM Plex, and real spectrogram language differentiate it.

---

## Direction B — "Oscilloscope" (refined dark, phosphor)

`direction-b-oscilloscope.html`

### Colors (hex)
| Token | Value | Use |
|---|---|---|
| scope background | `#0A0E0C` | page background (very dark, faintly warm green-black) |
| panel | `#11150F` | cards / panels |
| bone text | `#E6E1D6` | primary text |
| dim text | `#8A9285` | secondary text, captions |
| phosphor green | `#6FCF73` | single signal accent: traces, links, headings accents, buttons |
| amber | `#E8A33D` | ONLY tiny live/status indicators (blinking dot, metric placeholders) |
| scope grid | `rgba(111,207,115,0.08)` | faint 28px background grid |
| hairline | `rgba(111,207,115,0.18)` | rules, panel borders |

NO violet, NO cyan, NO glassmorphism / backdrop-blur, NO gradient orbs.

### Fonts (Google Fonts)
- Headings: **Space Grotesk** — 500, 700
- Body: **Space Grotesk** — 400
- Mono / data / status readouts: **Space Mono** — 400, 700

### Layout language
- Grid-locked, instrument-panel feel. Thin phosphor rules. 1100px max width.
- Mono status readouts instead of numbered labels: `STATUS: AVAILABLE`, `CH1 · 2.00 V/div · 5 ms/div · TRIG ▲`, `UPTIME 4+ YRS`, `SWEEP: 3 SIGNALS CAPTURED`, `COMMS: OPEN`, coordinate-style `SIG ID`.

### Signature element
- **Oscilloscope hero**: name + tagline sit over a live-drawn green phosphor waveform on a scope grid (`<canvas id="scope">`, DPR-aware, full-bleed). Multi-harmonic sine sweep with a focused phosphor glow on the trace line only (`shadowBlur` on the stroke — on-brand, NOT a banned ambient orb). Center axes brightened. Reduced-motion renders a static frame.
- Blinking amber **live dot** (`STATUS: AVAILABLE`, `COMMS: OPEN`).
- **Projects as instrument "channels"**: each is a panel with a left mini-scope screen + readout. Tiny generated trace per channel:
  - CH·A (Singing Emotion) = audio envelope waveform
  - CH·B (Malware CNN) = digital/square-pulse trace
  - CH·C (MNIST) = latent scatter dots
  - Each has tags, a mono readout line with placeholder metric, and a link.
- Portrait gets a subtle scanline overlay + luminosity blend to sit in the scope aesthetic.

### Deliberately avoided
The violet `#7c3aed` / cyan `#22d3ee` palette, frosted-glass cards, and big blurred background orbs the current site uses — deliberately left behind.

---

## Shared quality floor (both files)
- Responsive to ~360px: hero scales with `clamp()`, grids collapse to one column, nav becomes a Menu toggle.
- Visible keyboard focus: `:focus-visible` outline in the accent color on all links/buttons.
- `prefers-reduced-motion: reduce`: all canvas animations render a single static frame; entrance/transition motion disabled.
- Real anchor nav; real links — email `mailto:ibtihajtahir01@gmail.com`, GitHub `https://github.com/IbtihajT`, LinkedIn `https://www.linkedin.com/in/muhammad-ibtihaj-tahir`.
- No external JS libraries — vanilla canvas/SVG only.

## Content (both, real — no generic filler)
Name, roles, tagline, about (4+ yrs / 10+ projects), 7 skill tags, 3 services (Data Engineering / ML-AI Development / MLOps & Deployment with their real bullets), 3 real projects as case studies/figures, stack in 2 groups (Core Languages & Data; ML & DL Frameworks), contact heading "Let's Build Something Intelligent Together".
