# Shard Scramble

A real-time multiplayer arena game for 2–8 players. Open the page on any phone or laptop, create a room, and share the 4-letter code. No login, no install.

## How to play

1. Move with **WASD / arrow keys** (phone: drag on the screen).
2. Touch **blue shards** for +1 point and rare **gold shards** for +3.
3. Press **Space** (phone: DASH button) to dash. Dashing into a rival makes them **drop half their shards** on the floor, and anyone can grab them.
4. Dash has a 2-second cooldown (the ring around your player). After you get hit you flash and are safe for 1.5 seconds.
5. A round lasts **90 seconds**, and the last 15 seconds are a *Shard Frenzy* with faster spawns. Most shards wins.

## How it works

- **Single static page** (`index.html`): HTML, Canvas and vanilla JS, hosted on GitHub Pages.
- **Host-authoritative sync:** the player who creates the room runs the game simulation at 60 Hz in their browser. Other players send their inputs, and the host broadcasts the game state 20 times a second.
- **Transport:** messages go through a free public MQTT broker over secure WebSockets (`broker.emqx.io`). The first version used WebRTC (PeerJS), but peer-to-peer connections failed on networks that block STUN/UDP. Moving to a WebSocket relay fixed that.
- **Client-side prediction:** your own player moves the moment you press a key. The game then corrects toward the host's position by replaying any inputs the host hasn't processed yet, which hides the round-trip delay (about 0.5 s on some networks).

## Run locally

```bash
python3 -m http.server 8765
```

Open http://localhost:8765 in two tabs (or on two devices) to play.
