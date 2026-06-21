# Snakes & Ladders (Moksha Patam)

A real-time multiplayer Snakes & Ladders game with classic board layout, SVG snake/ladder overlay, and room-based online play via WebSockets.

**Live URL:** https://snakeladder.cursedshrine.com

---

## Features

- **Online multiplayer** — create or join rooms via invite link (`?room=ID`)
- **Local pass-and-play** — 2–4 players on the same device
- Server-authoritative dice rolls (no client cheating)
- Animated SVG snakes and ladders on the board
- 2–4 player support with colour-coded tokens
- Mobile responsive

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | Node.js + Express 5 |
| Real-time | Socket.io |
| Frontend | Static HTML/CSS/JS |
| Hosting | Raspberry Pi → Cloudflare Tunnel |

## Game Rules (Board Layout)

**Ladders (bottom → top):** 1→38, 4→14, 9→31, 21→42, 28→84, 36→44, 51→67, 71→91

**Snakes (head → tail):** 16→6, 47→26, 49→11, 56→53, 62→19, 64→60, 87→24, 90→48, 93→73, 95→75, 98→78

## Project Structure

```
snake-ladder/
├── server.js          # Express + Socket.io server, game logic
└── public/
    └── index.html     # All frontend (HTML/CSS/JS in one file)
```

## Running Locally

```bash
npm install
node server.js         # starts on :4177
```

## Deployment

```bash
sudo systemctl status snake-ladder
sudo systemctl restart snake-ladder
```

Port `4177` → Cloudflare Tunnel → `snakeladder.cursedshrine.com`.
