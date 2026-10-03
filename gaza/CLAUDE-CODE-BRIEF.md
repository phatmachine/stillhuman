# Seeing is Believing: build brief for Claude Code

Owner: Mark (mark@thecode.co.nz) · Prepared 3 October 2026 · Status: ready to build

This pack turns the approved prototype into a production site. Read this file first, then `CLAUDE.md` (already at the root of this pack; also reproduced in section 12), then `comparison-playbook.md`.

## 0. Files in this pack

| File | What it is |
|---|---|
| `README.md` | Start here: what's in the pack and the kickoff prompt. |
| `CLAUDE.md` | Standing rules for Claude Code; copy to the repo root. |
| `CLAUDE-CODE-BRIEF.md` | This brief. |
| `prototype/scroll.html` | The approved reference prototype: a single self-contained page. Match its behaviour and copy; rebuild its structure properly. |
| `data/metrics.json` | Every number on the site, with source URL, date and source tag. **The only place numbers live.** |
| `data/images.manifest.json` | Image slots, licence rules, verification fields. |
| `data/owners.json` | Largest shareholders and divestments per company, with sources. |
| `images/` | Section background photos (Children, Homes, Hospitals), 2000px JPEG. |
| `comparison-playbook.md` | Editorial rules: the your-world vs Gaza pattern, colours, accuracy rules. |
| `cro-review.md` | Why each metric is presented the way it is (stat → context → meaning). |

## 1. What we are building

A scroll-told story on a dark ground, with one subject per screen. The subjects are Children, Kindergartens, School, Homes, Lives, Hospitals, Medical evacuation, Water, Injuries and Bombardment. Each subject is pinned for about 4.4 screen-heights of scroll and told in **three steps that replace one another**, so nothing competes with the number:

1. **The number, alone.** A plain one-word label (e.g. "KINDERGARTENS"), the primary metric set huge (96–240px) and counting up, one plain sentence saying what it is, and the source. Nothing else is on screen.
2. **The comparison.** It replaces step 1: one plain sentence, then **inanimate forms only**: horizontal bars, block grids (one block = one unit) or week squares, drawn as you scroll.
3. **What it means.** One sentence, then two or three supporting figures, with "What Israel says" where relevant.

User testing showed that secondary numbers and illustrated icons diluted the primary metric. **No pictorial icons, and no secondary figures beside the primary number.**

**Order:** intro → "We let this happen." (satellite) → Children → Kindergartens → School → Homes → Lives → Hospitals → Medical evacuation → Water → Injuries → Bombardment → companies → your choice → record.

**Always-available exit:** a fixed button, "Who supplied it ↓", sits bottom-centre on every screen until the companies section is reached, then hides. It lets a reader who finds the content too much jump straight to the companies at any point. Keep it visible on phones, clear of the safe-area inset.

Then come the companies, "Your choice" and the court record, as normal scrolling sections. The record lists findings **newest first**.

**Reference site.** The owner pointed to an archived version of emmpo.com (Dec 2025). The archive copy could not be opened from this session. The live domain now hosts an unrelated investment site and must not be used as a reference. **Ask Mark for screenshots or a screen recording of the archived page before polishing motion.** The pattern described above is what he asked for: full-screen subjects with scroll-driven transitions into the detail.

## 2. Recommended stack

- **Astro 5 + TypeScript** for a static, fast, content-first site. One page component per chapter; data imported from `metrics.json`. (Next.js static export is an acceptable alternative if the team prefers React.)
- **GSAP 3 + ScrollTrigger** for pinning and scroll-scrubbed timelines.
  - Use `scrub: true`, `pin: true`, and one timeline per chapter, with labels for each beat.
  - GSAP and its plugins have been free for commercial use since 2025. **Confirm the current licence at gsap.com before shipping.**
- **Lenis** (npm `lenis`) for smooth scrolling, synced with ScrollTrigger per Lenis's GSAP recipe.
  - Disable it for `prefers-reduced-motion`.
  - Keep native touch scrolling on phones (`syncTouch: false`).
- **No canvas library is needed.** The 21,283-dot wall is a plain `<canvas>`, as in the prototype.
- **Visual forms**: bars, block grids and week squares only; no pictorial icons in the data story. Phosphor (MIT) supplies the utility icons (envelope, copy, external link).
- **Fonts**:
  - **Primary title headers** ("SEEING IS BELIEVING.", "WE LET THIS HAPPEN.", section titles such as "THESE COMPANIES MADE THE MACHINES"): **TRT Burn**, uppercase, **line-height 1.1**. The CSS token is `--title: "TRT Burn", "Anton", Impact, sans-serif`.
  - **Licence first:** the free download at dafontfree.io is marked **personal use only**. This is a public campaign site, so buy the commercial licence, with **web-font / website embedding rights**, from Creative Fabrica (the purchase link is on the dafontfree page) before launch. Keep the licence receipt in `/docs/licences/`.
  - **Install:** convert the licensed TTF to WOFF2 (e.g. `pip install fonttools brotli` then `pyftsubset TRTBurn.ttf --flavor=woff2 --unicodes=U+0020-007E,U+2018-201D,U+2014 --output-file=public/fonts/TRTBurn.woff2`). Self-host it at `/fonts/TRTBurn.woff2` with `font-display: swap`. Until it is in place, the titles fall back to Anton.
  - **Big numbers:** Anton.
  - **Statements and body text:** Archivo 400–900, via `@fontsource/anton` and `@fontsource/archivo`.
  - No serif and no italic anywhere.
- **Hosting**: Cloudflare Pages, Netlify or Vercel, all static. Add a strict CSP.
- **Analytics**: privacy-respecting only (Plausible or Cloudflare Web Analytics). No ad trackers.

## 3. Suggested structure

```
src/
  data/metrics.json            ← copy from this pack; single source of truth
  data/images.manifest.json
  lib/metrics.ts               ← typed getters: m('homes_damaged').value.share
  lib/format.ts                ← number/percent formatting, "1 in N"
  components/
    Chapter.astro              ← pinned stage with three steps (number → comparison → meaning)
    SectionBg.astro            ← greyscale 30% background photo + credit; reads manifest by slot id
    SatelliteWipe.astro        ← full-screen Rafah before/after, "We let this happen."
    BigNumber.astro            ← counting primary metric
    Bars.astro                 ← horizontal comparison bars
    Blocks.astro               ← block grid, one block = one unit
    Weeks.astro                ← week squares
    ChildrenWall.astro         ← canvas wall of 21,283 squares + counter
    Meaning.astro              ← meaning line, supporting figures, source tags
    JumpButton.astro           ← fixed "Who supplied it ↓"
    SourceTag.astro            ← UN · Ref · Gaza · Israel badge
    IsraelSays.astro           ← "What Israel says" disclosure
    Companies.astro, YourChoice.astro, Record.astro
  scripts/scroll.ts            ← GSAP timelines per chapter
  icons/                       ← Phosphor utility icons + flag sprite only
  pages/index.astro
tests/
  data.spec.ts                 ← every rendered number maps to a metric id
  scroll.spec.ts               ← Playwright: screenshots at beats
  links.spec.ts                ← all source URLs return 200
```

## 4. Chapter specification

Progress `p` runs 0 → 1 across a chapter (440vh by default; 320vh for Children). The step windows:
- **Step 1:** visible for p 0–0.22. The number counts up during p 0–0.06.
- **Step 2:** visible for p 0.25–0.48. Bars and blocks draw during p 0.27–0.42.
- **Step 3:** from p 0.52 to the end. This is the **longest hold, about two screens**, because testers needed time to absorb the meaning before it scrolled away.
- **Step indicator:** three bars, bottom left.

| # | id | Step 1: primary metric | Step 2: comparison form | Step 3: supporting figures |
|---|---|---|---|---|
| 1 | children | 21,283 children killed, identified by name (counter + 21,283-square wall) | the wall | 12,300 in 4 months vs 12,193 worldwide in 4 years (caption) |
| 2 | kindergartens | 86% destroyed, damaged or cut off | bars: 89% (OECD preschool) vs 14% (kindergartens usable) | 563 kindergartens; 70,000 children |
| 3 | school | 154 weeks without regular classes | 564 blocks, 526 red (school buildings to rebuild) | 657,200 children (close to every pupil in Scotland); classes restarted Sep 2026 in tents |
| 4 | homes | 371,888 of 485,361 homes (76.6%) | 100 blocks, 77 red | 82% of all buildings (UNOSAT); 65% in tents |
| 5 | lives | 73,922 killed | 30 blocks, 1 red | 174,995 injured; 58,000+ lost a parent |
| 6 | hospitals | 42 of 670 fully working | 670 blocks: 355 not working, 273 partly, 42 fully | 1,006 attacks; 404 ambulances; 46% medicines at zero |
| 7 | evacuation | 1,092 died waiting | bars by country (blue = Europe and N. America) | 18,500 needing care; 6,121 evacuated on WHO missions |
| 8 | water | 31% of households below 15 L/day | bars: UK 142 L, Germany 121 L, minimum 15 L | 57% below 6 L drinking; 91% water-insecure |
| 9 | injuries | 43,011 life-changing injuries | bars: needed 32,835 vs delivered 12,146; amputees 2,277 vs fitted 502 | 0 rehab equipment shipments; 526-day wait |
| 10 | bombardment | 6,000 bombs in 6 days (Israeli Air Force) | bars: 6,000 (Gaza, 6 days) vs 7,423 (Afghanistan, 2019) | 40,000+ targets; under 40% of land reachable |


### The satellite section: "We let this happen."

**This comes first**, straight after the intro and before Children. Its id is `satellite` and its scroll length is 420vh.

**Title:** "Gaza – Rafah from space".

**The images**
- The before/after comparison is **full screen**: the image fills the whole pinned stage (100vw × 100svh, `background-size:cover`) for the entire scroll through the section.
  - Overlaid on the image: the title and the before and after dates at the top, on a dark gradient; the Before and Now tags; "We let this happen." at the bottom.
  - The bottom padding (about 104px) keeps the heading clear of the fixed "Who supplied it ↓" button.
- **Area:** the same area of **Rafah** at the same zoom:
  - **Before:** before October 2023.
  - **After:** now.
- **Source:** Google Maps satellite view, at the owner's link https://maps.app.goo.gl/ZG2dKQuc7gzBPpsdA. Google Maps shows only current imagery, so the "before" image has to come from Google Earth Pro's historical imagery, or from Copernicus Sentinel-2 at 10 m per pixel (free, with credit).
- **Credit:** a tiny line under the heading reads "Imagery: Google Maps", linked to that URL and opening in a new tab. Google's attribution guidelines also require the imagery providers shown in the product (for example "Imagery ©2026 Airbus, Maxar Technologies"), so add the provider line exactly as Google shows it for each capture. Record both captures in `images.manifest.json` (slots SAT-BEFORE and SAT-AFTER) with their imagery dates.
- The "Preview with your own two images" panel sits just below the section. It is for the prototype only and can be removed once the real images are in.

**Behaviour**
- Scroll drives a wipe from before to after (p 0.12–0.52). The reader can grab the line and drag it at any point, and dragging overrides the scroll. The handle is keyboard-operable with the arrow keys.
- At p ≥ 0.58 the words **"We let this happen."** rise over the "after" image. This is the owner's chosen wording. Keep it exactly.
- The top bar shows both imagery dates. Replace the placeholder dates (1 Oct 2023 / 2026) with the real capture dates.

**Verification**
- Both images come from the same sensor, at the same area and zoom.
- Record the acquisition dates and tile IDs in the manifest.
- Confirm no cloud cover over the area shown.

**Motion rules**
- Bars scale from the left, and blocks and week squares light in order, all tied to scroll.
- The children counter and the wall are tied to scroll. The easing starts slowly and keeps climbing; it must never ease out to a stop early.
- **Small screens:** when a stage is taller than the viewport, glide its content upward between p 0.5 and 0.8 so the meaning comes into view (see `--shift` in the prototype). Never clip text.
- **Chapter rail** (right edge, desktop) and a 3px progress bar (top) show where the reader is.
- **`prefers-reduced-motion`:** no pinning, no scrub, all states shown filled, and the sections stack as a normal page.

## 5. Data rules (non-negotiable)

1. **No figure is typed into copy.** Copy uses tokens, e.g. `{homes_damaged.share|pct}`, resolved at build. `tests/data.spec.ts` fails the build if a digit sequence in rendered text has no matching metric.
2. **Every metric has** `source.url`, `as_of` and `tag`. Entries with `"verify": true` must be checked against a primary source, and their `TODO` URLs replaced, before launch:
   - `bombs_first_6_days`: Israeli Air Force statement, 12 Oct 2023.
   - `us_afghanistan_2019_weapons`: US Air Forces Central airpower summary, 2019.
3. **Rows that measure different things carry their own unit labels**, e.g. preschool: children enrolled vs kindergartens usable.
4. **Legal findings are attributed** to the body that made them (ICJ, ICC, UN Commission of Inquiry, Amnesty, HRW). The site never declares guilt in its own voice.
5. **"What Israel says"** stays beside the wheelchairs, force and record sections.
6. **Updating:** the oPt Health Cluster dashboard (link in `metrics.json`) and the OCHA Snapshot are the main live sources. Add a monthly scheduled job, or a checklist, to refresh figures and the `compiled` date.

## 6. Imagery

Imagery helps, but only if it is **licensed and verified**. Follow `images.manifest.json`:
- **Free to use with credit:** Copernicus Sentinel-2 and UNOSAT maps.
- **Usable if the licence fits:** Wikimedia Commons images under CC BY, CC BY-SA or public domain.
- **Check each image's terms first, or ask for permission:** UN, UNRWA, WHO, UNICEF and Save the Children photos.
- **Never use without a paid licence on file:** wire and stock photos (Reuters, AP, AFP, Getty, Magnific/Freepik).
- **Verify every photo** for date, place and original publisher, using reverse-image search and geolocation. Record who checked it and when.
- **Dignity:** no graphic injuries, no bodies, and no identifiable children without the publisher's stated consent.
- **The build fails** if a page uses an image id that isn't `approved`.
- **Section backgrounds (IMG-CHILDREN-BG, IMG-HOMES-BG, IMG-HOSPITALS-BG):** Children, Homes and Hospitals each have a full-bleed photo behind the whole pinned stage, under all three steps. Treatment: `background-size:cover`, `filter:grayscale(1)`, `opacity:.3`, `aria-hidden="true"`. Each has a small credit at the top left: "Photo: NAME / Unsplash". All three are Unsplash License photos from Mohammed Ibrahim and Emad El Byed, with source URLs in the manifest. Before launch:
  - **Children:** the faces are identifiable, and the Unsplash licence carries no model release. Get consent from the photographer, or blur the faces.
  - **Homes:** confirm that the file matches its source URL.
  - **Hospitals:** the photo shows a city strike, not a hospital, so never caption it as a hospital.
  - Serve all three as AVIF/WebP, ≤ 250 KB each; they are already greyscale, so they compress well.
- **Formats:** AVIF/WebP with a JPEG fallback, `srcset`, title-card images ≤ 250 KB, lazy-loaded except the first.

## 7. Companies and "Your choice"

- **Company register:** `CO` array in the prototype script. Move it to `data/companies.json`, with fields: name, country (flag), category, status (documented / reported / contested), role, evidence, contact routes.
- **Contact routes:**
  - **Verified contact pages:** Caterpillar, Boeing, RTX, Rheinmetall and General Dynamics (media contacts).
  - **Official homepage only:** the others. Verify and upgrade them to contact pages where possible.
- **No logos:** names and national flags only (flags are in the prototype's SVG sprite).
- **Largest owners:** each card lists its largest disclosed shareholders, from `data/owners.json`. Each entry has:
  - holders and their % (or % of votes);
  - an "as of" range;
  - source links: SEC 13G/13D filings, proxy statements, UK and German holding notices, Consob, or dated 13F aggregates;
  - where they exist, **Divested** links to funds that publicly excluded or sold the company over Israel or the occupied territories (Caterpillar: Norway GPFG, KLP, ABP; Elbit: Norway GPFG, Swedish AP1–AP4; Palantir: Storebrand).
- **Ownership note:** a note above the filters counts how many companies BlackRock, Vanguard and State Street appear in. Compute it from the data; never type it. The note tells readers their pension may hold these companies.
- **Refresh owners quarterly:** 13F filings are due 45 days after each quarter end.
  - Vanguard reorganised in January 2026; cite Vanguard Capital Management filings, not older "The Vanguard Group" figures.
  - Before launch, check the US percentages against each company's 2026 proxy-statement 5% table. The sources could only be read through filings and aggregators.
  - Several aggregator figures are undated (MarketScreener); replace them with filing-based figures where possible.
- **Deliberately left out:**
  - **Lockheed Martin:** State Street's 14.29% may include employee-plan trustee shares.
  - **General Dynamics:** the Crown family link to Longview is not confirmed by the 2026 filings.
  - **Norway GPFG and BAE / General Dynamics:** those exclusions were over nuclear weapons, not Gaza, so they are not shown as divestments.
- **Card action:** each card ends with a link to the company's own contact page or website, labelled with the company name, e.g. "Caterpillar contact page". It opens in a new tab. There is **no pre-written letter or email anywhere on the site**; the owner removed it to reduce litigation risk.
- **The site collects nothing.**

### Section 12: "Your choice" (replaces the letter composer)

- **Purpose:** frame acting as a **personal moral choice** that the visitor makes for themselves, then show them how. The site states facts and attributes findings. It never tells people what to conclude about a named company, and it never puts words in their mouth to send.
- **Content, in order:**
  1. Kicker "12 · Your choice". Title "What you do now is your choice." (TRT Burn). Then a short lede.
  2. **Pull quote:** "…silence would have made me feel guilty of complicity." attributed to Albert Einstein, 1954.
     - Use these genuine words. The popular version, "If I were to remain silent, I'd be guilty of complicity", is widely regarded as misattributed, so don't use it.
     - Before launch, confirm the 1954 primary source and add it to the record's source list. It is quoted in Cato Institute commentary, "The Stupidest Einstein Meme".
  3. **Five numbered steps:**
     1. Decide where you stand.
     2. Find out what your money supports: pensions, KiwiSaver, super, 401(k)s, index funds.
     3. Move it, if you choose to, pointing to the "Divested" examples on the cards.
     4. Tell the companies in your own words, through the contact links on the cards.
     5. Tell the people around you.
  4. **"Questions you can ask your pension provider or broker":** three neutral questions, with a "Copy these questions" button (clipboard, with a select-text fallback).
  5. **Disclaimer:** "This page is information, not financial or legal advice…"
- **Rail label:** "Your choice". The section id is `choice`.
- **Before launch:** have a lawyer review the companies section and this section for the jurisdictions you publish in. Wording is deliberately attributive ("documented", "reported", "the company disputes it").

## 8. Accessibility

- Every picture has `role="img"` and an `aria-label` stating the numbers in words.
- Keyboard: the chapter rail is focusable, and skip links jump to Companies and Your choice.
- Colour is never the only signal. Rows carry text labels and numbers, and the blue and red pair needs a 3:1 contrast check against the background.
- Run an automated axe check plus a manual screen-reader pass (VoiceOver and NVDA).

## 9. Performance budget

- Lighthouse mobile: Performance ≥ 90, CLS < 0.05, LCP < 2.5 s on 4G.
- Ship JS only for scroll, the wall, the satellite wipe and the copy button. Target ≤ 60 KB gzipped plus GSAP and Lenis.
- Redraw the canvas wall only when the drawn count changes, as the prototype does.

## 10. Testing

- **Playwright:** for each chapter, scroll to p = 0.02, 0.3, 0.65 and 0.9 and save screenshots at 1280×800 and 390×844. Fail on horizontal overflow or console errors.
- **Data test:** see 5.1.
- **Link test:** every `source.url` and every company contact URL returns 2xx.
- **Reduced-motion run:** emulate `prefers-reduced-motion: reduce` and confirm every state is visible.

## 11. Open items for Mark (before launch)

1. **TRT Burn:** buy the commercial licence with web-embedding rights and supply the font file. Until then, titles fall back to Anton.
2. **Rafah satellite pair:** supply the before image (pre-October 2023, from Google Earth Pro historical imagery) and the current image, same framing and zoom. Add Google's provider attribution line and the real capture dates.
3. **Children photo:** get consent from the photographer, or blur the faces (see section 6).
4. **Homes photo:** confirm that it matches its Unsplash URL.
5. **Bomb figures:** re-verify the two flagged figures (section 5.2).
6. **Motion reference:** screenshots or a recording of the archived emmpo.com page, for motion polish.
7. **Launch setup:** choose the domain, hosting account and analytics.

## 12. `CLAUDE.md` (also included as a file at the root of this pack)

```md
# Seeing is Believing

Scroll-told data story about Gaza. Read CLAUDE-CODE-BRIEF.md and comparison-playbook.md before changing anything.

## Hard rules
- Numbers come only from src/data/metrics.json. Never type a figure into copy or components.
- Every number shows its source tag (UN, Ref, Gaza, Israel). Keep "What Israel says" sections.
- Legal findings are attributed to the body that made them; the site never declares guilt itself.
- No company logos, no stock or wire photos without a licence on file, no image without an approved manifest entry.
- prefers-reduced-motion must show every state without pinning or scrub.

## Commands
- npm run dev / npm run build / npm run test (Playwright + data + link checks)

## Style
- Dark ground (#0c0d0f), bone text (#f2efe9). Gaza = red (#e5483b), comparison = muted blue (#6f9fd2), other = grey (#5d5f66).
- Three steps per subject: number alone, then comparison, then meaning. Never put secondary numbers beside the primary number.
- Inanimate forms only (bars, blocks, squares). No pictorial icons.
- The satellite section comes first. The "Who supplied it ↓" button must stay reachable until the companies section.
- TRT Burn (uppercase, line-height 1.1) for primary titles; licence with web rights required. Anton for big numbers, Archivo heavy for statements, Archivo for text. No serif, no italic. No pictorial icons.
- Children, Homes and Hospitals have a greyscale background photo at 30% opacity with a small Unsplash credit.
- The satellite section ("Gaza – Rafah from space") is full screen, credited "Imagery: Google Maps" with a link.
- The reference implementation is prototype/scroll.html: match its beats and copy.
```
