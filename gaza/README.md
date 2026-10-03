# Seeing is Believing: build pack for Claude Code

Prepared 3 October 2026 for Mark (thecode.co.nz). This pack holds everything needed to turn the approved prototype into a production website.

## How to use it

1. Unzip this folder and open it in a new, empty project directory. Copy `CLAUDE.md` to the repository root if it isn't there already.
2. Start Claude Code in that directory.
3. Paste the kickoff prompt below.

## Kickoff prompt

> Read README.md, CLAUDE.md, CLAUDE-CODE-BRIEF.md and comparison-playbook.md in full before writing any code. Then open prototype/scroll.html in a browser (with Playwright) and scroll through it at 1280×800 and 390×844, so you understand every beat.
>
> Build the production site described in the brief:
> - Astro 5 + TypeScript, GSAP ScrollTrigger and Lenis, static output.
> - Copy data/metrics.json, data/owners.json and data/images.manifest.json into src/data/. They are the only source of numbers and images.
> - Match the prototype's order, copy, three-step chapters, timings, the full-screen Rafah satellite section, the section background photos, the fixed "Who supplied it ↓" button, the companies section with its ownership data, and the "Your choice" section. There is no pre-written letter anywhere.
> - Set up the tests in section 10 of the brief (Playwright beat screenshots, the data test, the link test and the reduced-motion run). Make them pass.
>
> Work in this order: scaffold and data layer → chapter shell and scroll engine → each chapter → satellite section → companies and "Your choice" → accessibility and performance → tests. Show me a running dev build after the chapter shell works.
>
> Do not invent, round or restate any figure. If something in the brief conflicts with the prototype, follow the brief and tell me. List anything blocked by the open items in section 11 instead of guessing.

## What's inside

| Path | What it is |
|---|---|
| `CLAUDE.md` | Standing rules Claude Code reads every session. |
| `CLAUDE-CODE-BRIEF.md` | Full build brief: stack, structure, chapter spec, satellite spec, data rules, imagery, accessibility, performance, testing and open items. |
| `comparison-playbook.md` | Editorial approach and the comparisons in use. |
| `cro-review.md` | Background: why each metric is presented as it is. It predates later changes; where it differs from the brief, the brief wins. |
| `prototype/scroll.html` | The approved reference prototype (self-contained; open it in any browser). |
| `data/metrics.json` | Every figure, with its source, date and tag. |
| `data/images.manifest.json` | Image slots, credits, licences and launch status. |
| `data/owners.json` | Largest shareholders and divestments for each company, with sources. |
| `images/` | Background photos for Children, Homes and Hospitals (Unsplash License). |

## Still needed from Mark before launch

See section 11 of the brief:
- The TRT Burn web licence and font file.
- The Rafah before and after images, with Google's attribution line.
- Consent or face-blurring for the Children photo.
- Confirmation that the Homes photo matches its source.
- Re-verification of the two bomb figures.
- A final check of the shareholder percentages against each company's 2026 proxy statement.
- The emmpo.com motion reference.
- Domain, hosting and analytics.
