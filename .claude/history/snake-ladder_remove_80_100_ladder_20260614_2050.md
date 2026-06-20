# Snake & Ladder — Remove 80→100 Ladder (2026-06-14 20:50 IST)

## What changed
- Removed the `80: 100` entry from the `LADDERS` map. Square 80 is now a plain cell;
  the 80→100 ladder rail/rungs no longer render on the board overlay.

## Resulting layout
- Ladders: 1→38, 4→14, 9→31, 21→42, 28→84, 36→44, 51→67, 71→91 (8 total)
- Snakes (unchanged): 16→6, 47→26, 49→11, 56→53, 62→19, 64→60, 87→24, 90→48, 93→73,
  95→75, 98→78 (11 total)

## Effect
- 100 can no longer be reached via a shortcut from 80 — players must roll the exact
  number from their current position to land on 100, increasing the snake/ladder
  imbalance further in favor of snakes (11 vs 8), making the endgame harder.

## Deployment
- Static file edit only (`public/index.html`), served by `snake-ladder.service`
  (port 4177) — no restart needed, live immediately at
  https://snakeladder.cursedshrine.com
