# Favicon switched to the Shows / Cursed Shrine mark (2026-09-14 01:45)

- Asked: use the same favicon as shows-app in all UI apps (blog, TicTacToe, CursedCraft, Snakes & Ladders).
- Copied shows-app/public/favicon-mark.svg (tab icon) and logo-icon.svg (apple-touch-icon)
  into this project's public/ (byte-identical, same files the homepage uses).
- Added <link rel="icon" type="image/svg+xml" href=".../favicon-mark.svg"> and
  <link rel="apple-touch-icon" href=".../logo-icon.svg"> to the page head.
- public/index.html had no icon before. Served statically by server.js — no restart.
  Backup: .claude/backups/index.html.bak-favicon-20260914
