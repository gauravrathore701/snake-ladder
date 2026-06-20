# Snake & Ladder — Dice Matches Current Player's Color (2026-06-14 21:00 IST)

## What changed
- `#dice` background now switches to the current player's token color
  (set in `updateTurnInfo()`, which already runs on game start, after each turn,
  and after an undo)
- Added a `.25s` background transition on `#dice` for a smooth color swap
- Pip color (`#1a0e23`, dark) stays legible against all four bright player colors
  (`#ff5e5e`, `#5ee0ff`, `#ffd24d`, `#7CFC00`)

## Deployment
- Static file edit only (`public/index.html`), served by `snake-ladder.service`
  (port 4177) — no restart needed, live immediately at
  https://snakeladder.cursedshrine.com
