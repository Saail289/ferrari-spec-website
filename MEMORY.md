# Memory: lessons learned

A living log of mistakes, user corrections and what worked. Read it before every task. Add an entry whenever:
- the user corrects you,
- a change breaks something, or
- an approach works notably well.

Keep entries short. If an entry turns out wrong, edit or delete it rather than piling on.

Format: `- **YYYY-MM-DD: short title.** What happened → rule to follow now.`

## Mistakes to avoid

- **2026-10-28: Opening the page from file://.** support.js fetches `location.href`, so the page breaks without a server → always preview with `npx serve site`.

- **2026-10-28: Screenshotting the pinned era cards.** A plain headless screenshot only shows the hero, and headless rAF throttling leaves the scrub mid-fade → drive Chrome over CDP (a small Node script using the remote-debugging port), scroll, then call `st.getTween().progress(1)` before capturing.
- **2026-10-28: Parallel agents editing index.html.** One agent rewrote the whole file in one shot, which risked clobbering others' edits → when several agents share `index.html`, each uses small Edit replacements in its own section only, and the reviewer re-checks every section afterwards.
- **2026-10-28: Duplicate visuals across sections.** The Aerodynamics card prompt (wind tunnel + smoke) duplicated the new Galleria del Vento video → before writing a media prompt, check that no other section already uses that motif.
- **2026-10-28: Commons API 503s.** `commons.wikimedia.org/w/api.php` and `rest.php` returned 503 all session, and thumbnails only come in standard widths (960/1280/1920…, else HTTP 400) → read category HTML and `index.php?action=raw` for licences, and download 1920px thumbs, then resize locally.

- **2026-10-28: Gemini images carry a sparkle watermark.** User-supplied AI images had a ✦ at about (1100,795) on 1200×896 → check every corner before use. Remove it with `cv2.inpaint` (TELEA) on a star-shaped mask. A cloned patch left a visible square on textured floors.

## What works

- **2026-10-28: Hero knockout.** A multiply-blend layer with white type over video (`data-knock-layer`) is the signature moment. Don't touch it unless asked.

## User preferences

- Wants a professional, artistic, high-end result, not a template look.
- Uses fal.ai for generated images and video (key in `.env`).
