# Removed paper fold-crease lines (2026-09-13 15:00)

## Asked
- Gaurav saw a faint dark line down the middle of the page; remove it from every project.

## Cause
- The blog-derived paper background has two "fold crease" gradient layers:
  vertical hairlines at 22% / 57% / 81% width and a horizontal one at 63% height
  (rgba(90,70,40,~0.03)). The 57% one reads as a line through the middle.

## Changed
- Deleted both crease layers (`linear-gradient(90deg …)` and `linear-gradient(0deg …)`)
  from the body background. Fibre grain + vignette kept, so the paper look stays.
- Applied identically on cursedshrine.com homepage, TicTacToe (src/index.css, rebuilt dist)
  and Snakes & Ladders (public/index.html). No service restarts needed.
- NOT changed: bloging-app (blog still has the creases) — awaiting Gaurav's call,
  prod deploy needs his explicit go.

## Backups
- homepage: .claude/backups/index.html.bak-20260913-1500
- snake-ladder: .claude/backups/index.html.bak-20260913-1500
- TicTacToe: .claude/backups/src-20260913/index.css.bak-1500

## Verified
- Live HTML/CSS of all three no longer contain the crease stops; headless screenshots clean.
