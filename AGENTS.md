# SuplimForge — working rules

Static site on GitHub Pages (suplimforge.com). Pushing to `main` publishes it
within a few minutes. No build step, no dependencies.

The owner converses in Thai. Files and commit bodies may be English; commit
titles follow the existing Thai style (`เพิ่ม: ...`, `แก้: ...`).

## Weekly devlog post (`devlog/index.html`)

One page, all posts, newest on top. Each post is one `<article id="wNN">`
covering one ISO week (Mon–Sun). Copy the structure of an existing article.

### Sources (the game repo `sp-arpg`, a sibling checkout)

- `UPDATES.md` — one line per player-visible change, by date. Main source.
- `media/updates/<date>-<slug>.png|gif` — the matching screenshots / GIFs.
- `journey/stories/<YYYY-Www>.md` — internal Thai week story. Background
  only: never copy it as-is, it is written for the team.

Pull `sp-arpg` first so you see the latest week.

### Steps

1. Pick the week. If it has no player-visible change in `UPDATES.md`, skip it
   — no filler posts.
2. If the previous post is marked "this week" (`<span class="live">`),
   finish it: add the rest of its week and remove the `live` spans.
3. Write the new article at the top, and add `<a href="#wNN">WNN</a>` first
   in `.weeks` in the header.
4. **Every piece of text exists twice**: one element with `lang="th"` and one
   with `lang="en"` (h2, h3, p, ul, figcaption). The TH/EN toggle hides the
   other. A missing twin shows nothing in that language.
5. Shape: `meta` line (`2026 · WNN · d Mon – d Mon`), an `h2` title, 2–4
   `h3` sections for the biggest changes with media, then "อื่น ๆ สัปดาห์นี้ /
   Also this week" as a short list. Short, plain, player-facing.
6. Media: 2–6 items per post, into `devlog/media/` with the same file name.
   - PNG: copy as-is.
   - GIF: do not publish the GIF. Convert:
     ```
     ffmpeg -i X.gif -c:v libvpx-vp9 -b:v 0 -crf 36 -row-mt 1 -pix_fmt yuv420p X.webm
     ffmpeg -i X.gif -movflags +faststart -pix_fmt yuv420p -vf "scale=trunc(iw/2)*2:trunc(ih/2)*2" -c:v libx264 -crf 24 -preset slow X.mp4
     ffmpeg -i X.gif -frames:v 1 -update 1 X-poster.jpg
     ```
     and use `<video poster=... autoplay muted loop playsinline preload="metadata">`
     with a webm `<source>` first and the mp4 second.
   - Prefer shots without big debug panels.

### Never publish

- Story, lore, characters, setting details, or game-name candidates. The
  owner will say when something is ready to tell.
- AI tools, models, sessions or internal workflow; internal bugs (saves,
  tests), debug/test tools and panels.
- Plans or ideas that are not in the game yet.

Names and numbers are placeholders; the header already says so — don't
repeat it in every post. Use the game's own terms: katha (คาถา) for supports,
spells (เวท), yantra (ยันต์), Tempo. The project is called "Project sp-arpg".

### Check before pushing

- Open `devlog/index.html` in a browser: both `?lang=th` and `?lang=en`, at
  desktop and phone width (~390 px). No broken image, videos play, no
  horizontal scroll.
- Show the owner the Thai text (or a screenshot) and push only after they
  say OK.
- Commit `เพิ่ม: Devlog WNN`, push `main`, then confirm the
  "pages build and deployment" run on GitHub succeeded.
