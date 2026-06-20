# Snake-Ladder — Real-time Online Multiplayer
**Date:** 2026-06-17 20:40

## What Was Built

Added real-time online multiplayer (2–4 players) with invite links, preserving the existing local pass-and-play mode fully intact.

## Architecture

- **New `server.js`** — Express 5 + Socket.io backend (CommonJS)
  - Serves `public/` static files
  - Hosts Socket.io WebSocket endpoint
  - In-memory room management (`rooms` Map)
  - Server-authoritative: dice rolled server-side, positions validated server-side
  - `animHint` in game-state tells clients which token to animate and how

- **New `package.json`** — `npm init` + `express` + `socket.io`

- **Rewritten `public/index.html`** — four new screens added, local mode fully preserved

## New Screens

1. **Mode Select** (shown at `/`) — "Local Play" | "Play Online" buttons
2. **Online Setup** (host flow) — pick 2/3/4 players, enter name, "Create Room"
3. **Join Screen** (shown when URL has `?room=abc12345`) — enter name, "Join Game"
4. **Waiting Room** — shows invite URL + copy button, live player slot list, waits for all N to join then auto-starts

## Online Game Flow

1. Host clicks "Play Online" → picks count → enters name → "Create Room"
2. Server creates room, assigns host as slot 0 (red, X)
3. Waiting room shows `snakeladder.cursedshrine.com?room=abc12345` with copy button
4. Guests open link → enter name → join → assigned next color slot
5. When all N players joined → server sets status: 'playing' → all clients auto-transition to game
6. Each player can only click ROLL on their turn (button disabled otherwise)
7. Server rolls dice, computes move (including snakes/ladders), broadcasts `game-state` with `animHint`
8. All clients animate the token step-by-step using animHint, then apply the final position

## Socket Events

| Event | Direction | Purpose |
|-------|-----------|---------|
| `create-room` | client→server | Host creates a room (maxPlayers, name) |
| `join-room` | client→server | Guest joins with (roomId, name) |
| `roll` | client→server | Active player rolls (server validates turn) |
| `play-again` | client→server | Reset board, keep players |
| `game-state` | server→clients | Full state broadcast after every change |
| `player-left` | server→clients | Notification when someone disconnects |

## Key Design Choices

- Dice rolled server-side (prevents cheating)
- `animHint: { slot, roll, fromPos, toPos, jumped }` — clients reconstruct step-by-step animation without needing extra round trips
- URL-based join: `?room=roomId` — no room code typing needed, just share a link
- Disconnected mid-game: player marked inactive, their turns skipped
- Undo button hidden in online mode (no undo in multiplayer)
- "Leave" button added to win modal for online mode
- Local mode: zero changes to game logic, only wrapped in mode-select flow

## systemd

Changed from:
```
ExecStart=/usr/bin/python3 -m http.server 4177 --bind 127.0.0.1 --directory .../public
```
To:
```
ExecStart=/home/gaurav/.nvm/versions/node/v24.13.0/bin/node server.js
```
