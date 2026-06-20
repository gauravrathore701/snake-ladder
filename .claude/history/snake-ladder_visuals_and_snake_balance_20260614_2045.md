# Snake & Ladder — Visual Polish & Snake Rebalance (2026-06-14 20:45 IST)

## Visual changes
- Board cell numbers: now white (`#ffffff`), bold, larger (`.55rem` → `.75rem`), with a
  subtle white text-shadow glow for readability against the dark theme
- Grid lines brightened from faint purple (`rgba(138,77,255,.12)`) to white
  (`rgba(255,255,255,.22)`)
- Even-cell shading switched from purple tint to white tint (`rgba(255,255,255,.07)`)
  for more contrast/vibrancy
- Board outer border/glow brightened: border color `#5a1fbf` → `#b48cff`,
  shadow intensified
- Start/goal cell highlights intensified slightly (start: green tint 10%→16%,
  goal: gold tint 18%→24% with gold text-shadow)

## Gameplay change
- Added a new snake: **90 → 48** (in addition to existing 93→73, 95→75, 98→78)
- This raises the snake density in the 91-100 endgame range from 3/10 to 4/10 squares,
  making the final stretch slightly more punishing/risky as requested

## Deployment
- Static file edit only (`public/index.html`), served directly by
  `snake-ladder.service` (port 4177) — no restart needed, change is live immediately
  at https://snakeladder.cursedshrine.com
