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
- No pre-written letters or emails to companies. Section 12, "Your choice", frames action as the visitor's own moral choice and explains how; keep its not-advice disclaimer.
- The reference implementation is prototype/scroll.html: match its beats and copy.
