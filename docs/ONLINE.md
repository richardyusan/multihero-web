# Online multiplayer (web version)

Up to **4 players** fight each other online, each in their own browser. Rooms are hosted by a small
[Cloudflare Worker](https://github.com/richardyusan/multihero-server) (one Durable Object per room);
the game itself runs in every browser.

Server: `https://multihero-server.multihero-web.workers.dev` (set in [`config/online.cfg`](../config/online.cfg)).

## How to play with friends

1. Press **▶ Play**, then **MULTIPLAYER ONLINE** in the main menu.
2. One player presses **CREATE ROOM**. The room gets a code such as `KX4P7`, shown in the game and in
   the bar at the top of the page.
3. Press **📋 Copy invite link** and send the link (`…/multihero-web/?room=KX4P7`) to your friends.
   Opening the link and then MULTIPLAYER ONLINE joins the room directly. Friends can also press
   **⌨ Join with code** (or the *enter room code* row) and type the code.
4. In the room everybody can pick a character (**[change]** under the name); the host picks the
   stage and the number of lives. The room shows each player's ping and READY state.
5. Everybody presses **READY**, then the host presses **START**. After a 3 second countdown the
   match starts on every screen at the same time.
6. After the match everybody comes back to the same room automatically, ready for the next round.

**👤 name** in the bar changes the name shown to the others (saved in the browser). Rooms you used
recently are listed in the room list. **ESC** twice during a match leaves it; the CPU plays on for
a player who leaves or loses the connection.

## How it works

```
browser A ──┐                                         ┌── browser B
 game + net │  wss://…/room/KX4P7   Cloudflare Worker │  game + net
 (lockstep) ├──────────────────────▶  Durable Object ◀┤  (lockstep)
            │   inputs, every 40 ms   "KX4P7" (relay) │
```

- **Lockstep**: every browser runs the same deterministic game (same WebAssembly, same random seed,
  same stage and characters). Only the buttons of each player are sent: a player's input for frame
  *F* is sent a few frames early (the *input delay*, 3–12 frames, chosen from the pings), and no
  browser simulates frame *F* before it has everybody's input for it. A checksum of the game state
  is compared every second; "out of sync!" is shown if two browsers ever differ.
  This part is shared with the Android port: `src/online.bb` (lobby screens, `np*` functions) and
  `runtime/net/net.cpp` (`NetPlay*`).
- **Backend `runtime/net/net_worker.cpp`** connects the lobby to the Worker:

  | Game | Worker |
  |---|---|
  | CREATE ROOM | new random 5-character code, WebSocket `wss://<server>/room/<CODE>` |
  | room list | "enter room code" + recent rooms (the server has no room list) |
  | names, characters, READY, ping | relayed message `{k:"hi"}` from every player |
  | stage / lives | relayed `{k:"room"}` from the host |
  | START | host sends `{k:"go"}`, everybody answers `{k:"fin"}` + `{t:"ready"}`; the server sends `start` with the shared seed when all are ready |
  | match inputs / checksums | relayed `{k:"g",a:[…]}`, several lockstep messages per packet |
  | ping | `"ping"` → `"pong"` (answered by the server without waking the room) |

- **Server limits** (`multihero-server/src/index.js`): 30 messages per second per player (burst 40)
  and 512 characters per message; messages over the limit are dropped. The lockstep code produces
  one input message per frame (54 per second), so the backend packs them into one packet every
  40 ms through a token bucket: about **20 packets per second**, at most ~130 characters each
  (measured in a test match).
- The **ping** shown for another player is the round trip through the server (their ping + yours),
  which is the delay their inputs really have. In the room list (no WebSocket yet) the ping is an
  HTTP request to the Worker.

## Tested

With the live server, using two and three headless Chromium browsers: create room, join by invite
link and by typed code, character / ready / start, 25–30 s matches at 52–54 fps without
desync, ~20 packets/s per player, a player leaving with ESC (CPU takes over), and both players
returning to the same room after the match. Not tested yet: real phones and slow mobile networks.

## Limitations / ideas

- No public room list or "players online" count: the server only knows room codes. A `/rooms`
  endpoint on the Worker (rooms report their name, host and player count to a lobby object) would
  allow a real room browser; the client would only need `refreshRooms()` in `net_worker.cpp`.
- A browser tab in the background is slowed down by the browser; the others then wait for it and,
  after 15 s without data, the CPU takes over that player.
- Web and Android players cannot play together (different server and protocol).
