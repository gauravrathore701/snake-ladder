# Snake & Ladder — Undo Last Move Button (2026-06-14 20:55 IST)

## What changed
- Added an "↺" undo button in the controls bar, between the dice and the ROLL button
- Before each roll, the current player's pre-move position is snapshotted
  (`lastMove = { playerIndex, prevPos }`)
- Undo restores that player's token to its pre-roll square, sets `current` back to
  that player (so it's their turn again), and clears the snapshot — single-level
  undo (can't undo more than the most recent move)
- Undo covers the whole turn including any snake/ladder resolution, since it
  reverts to the position before the dice roll was applied
- Button is disabled while a roll is animating, after it's been used, and once a
  player has won (re-enabled appropriately on next turn / reset via "Play Again")

## Deployment
- Static file edit only (`public/index.html`), served by `snake-ladder.service`
  (port 4177) — no restart needed, live immediately at
  https://snakeladder.cursedshrine.com
