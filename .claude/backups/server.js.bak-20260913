const express = require('express');
const { createServer } = require('http');
const { Server } = require('socket.io');
const path = require('path');

const app = express();
const httpServer = createServer(app);
const io = new Server(httpServer);

const COLORS  = ['#ff5e5e', '#5ee0ff', '#ffd24d', '#7CFC00'];
const LADDERS = { 1:38, 4:14, 9:31, 21:42, 28:84, 36:44, 51:67, 71:91 };
const SNAKES  = { 16:6, 47:26, 49:11, 56:53, 62:19, 64:60, 87:24, 90:48, 93:73, 95:75, 98:78 };

const rooms = new Map();

function makeRoom(maxPlayers) {
  return { maxPlayers, players: [], current: 0, status: 'waiting', lastRoll: null, lastMsg: '', animHint: null };
}

function snapshot(room) {
  return {
    maxPlayers: room.maxPlayers,
    players: room.players.map(p => ({ name: p.name, color: p.color, pos: p.pos, slot: p.slot, active: p.active })),
    current: room.current,
    status: room.status,
    lastRoll: room.lastRoll,
    lastMsg: room.lastMsg,
    animHint: room.animHint,
  };
}

io.on('connection', (socket) => {

  socket.on('create-room', ({ maxPlayers, name }, cb) => {
    const roomId = Math.random().toString(36).slice(2, 10);
    const room = makeRoom(maxPlayers);
    room.players.push({ socketId: socket.id, name: name || 'Player 1', color: COLORS[0], pos: 0, slot: 0, active: true });
    rooms.set(roomId, room);
    socket.join(roomId);
    socket.data.roomId = roomId;
    socket.data.slot = 0;
    cb({ ok: true, roomId, slot: 0 });
    io.to(roomId).emit('game-state', snapshot(room));
  });

  socket.on('join-room', ({ roomId, name }, cb) => {
    const room = rooms.get(roomId);
    if (!room) return cb({ error: 'Room not found' });

    const existing = room.players.find(p => p.socketId === socket.id);
    if (existing) {
      socket.join(roomId);
      socket.data.roomId = roomId;
      socket.data.slot = existing.slot;
      cb({ ok: true, slot: existing.slot });
      socket.emit('game-state', snapshot(room));
      return;
    }

    if (room.status !== 'waiting') {
      // Allow reconnect: take over an inactive slot (happens when player refreshes mid-game)
      const inactive = room.players.find(p => !p.active);
      if (!inactive) return cb({ error: 'Game already started' });
      inactive.socketId = socket.id;
      inactive.active = true;
      inactive.name = name || inactive.name.replace(' (left)', '');
      socket.join(roomId);
      socket.data.roomId = roomId;
      socket.data.slot = inactive.slot;
      cb({ ok: true, slot: inactive.slot });
      socket.emit('game-state', snapshot(room));
      return;
    }
    if (room.players.length >= room.maxPlayers) return cb({ error: 'Room is full' });

    const slot = room.players.length;
    room.players.push({ socketId: socket.id, name: name || `Player ${slot + 1}`, color: COLORS[slot], pos: 0, slot, active: true });
    socket.join(roomId);
    socket.data.roomId = roomId;
    socket.data.slot = slot;

    if (room.players.length === room.maxPlayers) room.status = 'playing';

    cb({ ok: true, slot });
    io.to(roomId).emit('game-state', snapshot(room));
  });

  socket.on('roll', () => {
    const { roomId, slot } = socket.data;
    if (!roomId) return;
    const room = rooms.get(roomId);
    if (!room || room.status !== 'playing') return;

    // Skip inactive slots to find current active player
    if (room.current !== slot) return;

    const value = 1 + Math.floor(Math.random() * 6);
    room.lastRoll = value;
    room.animHint = null;

    const p = room.players[slot];
    const fromPos = p.pos;
    const target = fromPos + value;

    if (target > 100) {
      room.lastMsg = `${p.name} rolled ${value} — needs exact roll to reach 100.`;
      room.animHint = { slot, roll: value, fromPos, toPos: fromPos, jumped: null };
      room.current = nextActive(room, slot);
      io.to(roomId).emit('game-state', snapshot(room));
      return;
    }

    p.pos = target;
    room.lastMsg = `${p.name} rolled ${value}.`;
    let jumped = null;

    if (LADDERS[p.pos]) {
      jumped = { from: p.pos, to: LADDERS[p.pos], type: 'ladder' };
      room.lastMsg += ` Climbs ladder! ${p.pos}→${LADDERS[p.pos]}`;
      p.pos = LADDERS[p.pos];
    } else if (SNAKES[p.pos]) {
      jumped = { from: p.pos, to: SNAKES[p.pos], type: 'snake' };
      room.lastMsg += ` Bitten by snake! ${p.pos}→${SNAKES[p.pos]}`;
      p.pos = SNAKES[p.pos];
    }

    room.animHint = { slot, roll: value, fromPos, toPos: target, jumped };

    if (p.pos === 100) {
      room.status = 'done';
      room.lastMsg = `${p.name} reached square 100 — Moksha!`;
      io.to(roomId).emit('game-state', snapshot(room));
      return;
    }

    room.current = nextActive(room, slot);
    io.to(roomId).emit('game-state', snapshot(room));
  });

  socket.on('play-again', () => {
    const { roomId } = socket.data;
    if (!roomId) return;
    const room = rooms.get(roomId);
    if (!room) return;
    room.players.forEach(p => { p.pos = 0; });
    room.current = 0;
    room.status = 'playing';
    room.lastRoll = null;
    room.lastMsg = '';
    room.animHint = null;
    io.to(roomId).emit('game-state', snapshot(room));
  });

  socket.on('disconnect', () => {
    const { roomId, slot } = socket.data;
    if (!roomId) return;
    const room = rooms.get(roomId);
    if (!room) return;

    const p = room.players.find(pl => pl.socketId === socket.id);
    if (!p) return;

    if (room.status === 'waiting') {
      room.players.splice(p.slot, 1);
      room.players.forEach((pl, i) => { pl.slot = i; pl.color = COLORS[i]; });
      if (room.players.length === 0) { rooms.delete(roomId); return; }
    } else {
      p.active = false;
      p.name += ' (left)';
      if (room.current === p.slot) room.current = nextActive(room, p.slot);
    }

    room.animHint = null;
    io.to(roomId).emit('game-state', snapshot(room));
    io.to(roomId).emit('player-left', { name: p.name });
  });
});

function nextActive(room, fromSlot) {
  const n = room.players.length;
  for (let i = 1; i <= n; i++) {
    const idx = (fromSlot + i) % n;
    if (room.players[idx].active) return idx;
  }
  return (fromSlot + 1) % n;
}

app.use(express.static(path.join(__dirname, 'public')));
app.get('/{*path}', (_req, res) => {
  res.sendFile(path.join(__dirname, 'public', 'index.html'));
});

httpServer.listen(4177, () => {
  console.log('Snake-Ladder server running on :4177');
});
