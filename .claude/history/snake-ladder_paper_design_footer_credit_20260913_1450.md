# Snakes & Ladders — Cursed Shrine paper design + footer credit (2026-09-13 14:50)

## Asked
- Apply the cursedshrine.com design (paper look from the blog) and the
  bottom-right "developed and managed by / With the help of Claudy Rex" credit.

## Changed
- `public/index.html`
  - Whole `<style>` block rewritten on the blog tokens (bg #e8dfc9, bg-sec #ddd2b7,
    ink #2b2723, accent #6b4a2f, line #cbbd9e, system sans) + same CSS paper texture.
    All existing selectors/IDs kept, so the game JS is untouched in behaviour.
  - Added shared chrome: `<header class="site-header">` (Cursed Shrine wordmark →
    cursedshrine.com, Portfolio, Blog) and `<footer class="site-footer">` (© year,
    Home/Portfolio/Blog, right-aligned credit linking gaurav.cursedshrine.com,
    small italic "With the help of Claudy Rex (AI Assistant)"). Content wrapped in `<main class="app">`.
  - Old "‹ Cursed Shrine" footer link replaced by the new footer.
  - Inline dark styles on name inputs / hint / error moved to classes
    (`.name-input`, `.hint`, `.error-text`).
  - Board colours for paper: ladders #8a6a4b, snakes #9b2d20 (eyes #f7f1e3),
    dice default bg accent, pips paper-white.
  - Player colours (client): #b23a2e, #2f6f8f, #b8860b, #4f7a2a (neon versions
    were unreadable as name text on paper).
- `server.js`: same COLORS array (online rooms assign colours server-side).
- `snake-ladder.service` restarted (0 established connections at the time).

## Backups
- `.claude/backups/index.html.bak-20260913`, `.claude/backups/server.js.bak-20260913`
  (git tree was also clean before the change).

## Verified
- Both inline scripts parse (`new Function`), `node --check server.js`, service active, 200.
- Headless screenshots: mode select (1280), local game after auto-rolls (1280 + 393) —
  board, snakes, ladders, tokens, dice, controls and footer credit all render.
