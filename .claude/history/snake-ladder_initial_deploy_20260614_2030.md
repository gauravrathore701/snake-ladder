# Snake & Ladder — Initial Deploy (2026-06-14 20:30 IST)

## What was built
A single-file HTML/CSS/JS implementation of Snakes & Ladders (Moksha Patam), the
ancient Indian board game, styled to match the Cursed Shrine purple/dark theme.

- 10x10 boustrophedon-numbered board (1 bottom-left → 100 top), rendered as a CSS grid
- Classic snake/ladder layout:
  - Ladders: 1→38, 4→14, 9→31, 21→42, 28→84, 36→44, 51→67, 71→91, 80→100
  - Snakes: 16→6, 47→26, 49→11, 56→53, 62→19, 64→60, 87→24, 93→73, 95→75, 98→78
  - Drawn as an SVG overlay (ladders = rails+rungs, snakes = wavy bezier path + head/eyes)
- Setup screen: choose 2-4 players, editable names, distinct token colors
- Turn-based local pass-and-play: animated dice roll (CSS pip faces), step-by-step
  token movement, ladder/snake resolution, exact-100 win rule (overshoot = no move)
- Win modal with "Play Again" reset
- Responsive, touch + mouse friendly (matches other Cursed Shrine subdomain games)

## Deployment
- Project: `/home/gaurav/Projects/snake-ladder/public/index.html`
- systemd service: `snake-ladder.service` — `python3 -m http.server 4177` (bind 127.0.0.1)
- Cloudflare tunnel ingress added: `snakeladder.cursedshrine.com` → `http://localhost:4177`
  in `~/.cloudflared/config.yml` (tunnel `sharemarketstudies-app`)
- DNS CNAME `snakeladder.cursedshrine.com` → `<tunnel-id>.cfargotunnel.com` created via
  Cloudflare API (zone `b9d4738cc0f4a264d49fbd63aa57ec70`), proxied
- `cloudflare-tunnel.service` restarted to pick up new ingress
- Verified: `https://snakeladder.cursedshrine.com/` returns 200

## Notes
- No build step / dependencies — single static HTML file, same pattern as
  `minecraft-clone` and `homepage-cursedshrine`.
- Port 4177 chosen (4174-4176 already in use by tictactoe/minecraft/homepage).
