# Ferrari Spec Website

A scroll-driven spec/tribute site for Scuderia Ferrari. Independent design concept, not affiliated with Ferrari S.p.A. Keep the footer disclaimer.

**Read [MEMORY.md](MEMORY.md) before starting any task.** It holds lessons from past mistakes and overrides general habits.

## Project layout

- `site/index.html`: the whole page (markup, inline styles, and the GSAP/Lenis logic in the `<script type="text/x-dc">` block).
- `site/support.js`: Claude Design runtime. Don't edit it. The page needs it to boot.
- `site/uploads/`: images and videos, including AI-generated assets.
- `site/screenshots/`: leftover Claude Design checks. Safe to ignore.
- Run locally: `npx serve site`, then open http://localhost:3000. `file://` won't work because support.js fetches the page.
- Deploy: drag the `site/` folder onto Vercel Drop or Netlify Drop.

## Design rules

Use the `frontend-design` skill (`.claude/skills/frontend-design`) for any visual work. On top of it:

1. **Ground every choice in the subject's real world**: Maranello, the paddock, carbon fibre, telemetry, Rosso Corsa paint, race livery, the Prancing Horse. Pick a motif from motorsport, never a generic SaaS one.
2. **Plan before building.** For any new section, write a short plan first: palette (named hex values), type roles, layout as an ASCII wireframe, and the one memorable moment. Check it against the skill's list of generic AI tells and revise anything that reads as default.
3. **Keep the existing system unless asked to change it.**
   - Colors: page `#0A0A0A`, text `#F5F3F0`, accent `--accent` (Rosso), yellow `#FFF200`, and era accents gold `#C9A24B`, rust `#C0632B` and red `#C81400`.
   - Type: Archivo 900 for display, JetBrains Mono for everything else.
4. **Spend boldness in one place per section.** One hero moment per section and quiet surroundings. Remove one accessory before shipping.
5. **Motion is the medium here, so make each effect earn its place.** Scroll-scrubbed (GSAP ScrollTrigger + Lenis), purposeful, and reversible. Don't add a new fade-up-on-every-section effect. Respect `prefers-reduced-motion`.
6. **Accuracy matters.** Car names, years, drivers and title counts must be factually correct. Verify with a web search when unsure.
7. **Quality floor:** responsive down to 375px, no horizontal scroll, visible keyboard focus, alt text on meaningful images, AA contrast for body text.
8. **Performance:**
   - Videos are MP4 (H.264), muted, `playsinline`, ideally under 4 MB each, with a poster frame.
   - Images are WebP/JPG sized to their display size.
   - Lazy-load anything below the fold.

## AI images and video (fal.ai)

- The key lives in `.env` as `FAL_KEY`. Never print it, log it, commit it, or put it in client-side code. Generation happens locally via scripts only.
- Look up the current best fal model for the job (image vs. video, stills vs. motion) before generating. Don't assume model IDs from memory.
- **House look for generated media:** cinematic, low-key lighting, deep blacks that blend into `#0A0A0A`, Rosso highlights, film grain, motion blur at speed.
  - No visible text in the frame. Garbled AI lettering ruins it.
  - No fake sponsor logos.
- Save outputs to `site/uploads/` with descriptive kebab-case names (e.g. `era-1950s-ascari.jpg`). Save prompts in `prompts/` (outside `site/`, so they don't get deployed) and log the model, date and settings in `prompts/README.md`. Keep uncompressed originals in `source-media/` and ship web-compressed copies: `ffmpeg -crf 26 -movflags +faststart -an`.
- Replace the `[ IMAGE — … ]` placeholders one at a time, screenshot, and judge them in context before moving on.

## Workflow

1. Read MEMORY.md, then the relevant part of `site/index.html`.
2. Make the change.
3. Verify visually. Take a headless screenshot or ask the user to check.
4. If something went wrong or the user corrected you, add a lesson to MEMORY.md before finishing.
