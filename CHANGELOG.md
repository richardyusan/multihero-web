# Changelog

## 1.3.0 – 2026-11-04
- **Real online multiplayer in the browser** (was a demo lobby): rooms on the Cloudflare Worker
  [multihero-server](https://github.com/richardyusan/multihero-server), up to 4 players, lockstep netplay.
  New backend `runtime/net/net_worker.cpp`; `config/online.cfg` points to the server.
- CREATE ROOM makes a room code; bar on the page with **Copy invite link** (`?room=CODE` joins
  directly), **Join with code** and the player name. Recent rooms in the room list.
- Room: names, characters, READY, ping per player; the host picks stage and lives and starts.
- Match inputs are packed into ~20 packets per second to stay under the server's 30 messages/s limit.
- After a match everybody returns to the same room automatically.

## 1.2.0 – 2026-11-04
Synced with the MultiHero Android port 1.7.0.
- Main menu **CHEATS** page: INFINITE HEALTH, INFINITE JUTSU (Adventure mode) and UNLOCK ALL CHARACTERS + MAPS as
  ON/OFF items you can click with the mouse.
- Multiplayer online menu: stage picker with pictures, player status with ping during a match, players who leave are
  taken over by the CPU (online play in the browser is still the demo lobby).

## 1.1.0 – 2026-11-03
Synced with the MultiHero Android port 1.6.0.
- New on-screen buttons **UP+SP** (up + special) and **DN+SP** (down + special), next to THROW / JUTSU.
- Main menu switches **INFINITE HEALTH** and **INFINITE JUTSU** for Adventure mode (each on/off, saved in the browser).
- **Several gamepads**: each pad controls its own player (first pad used = player 1, second = player 2 ... up to 4),
  also in the menus.
- Fixed: on-screen buttons stopped working after remapping keys in Options > Controls.
- Fixed adventure enemies that never attacked (Venom on stage 50, Shredder, Thug, Pig).
- MULTIPLAYER ONLINE menu (demo lobby with CPU opponents in the browser; real online play is in the Android app).

## 1.0.0 – 2026-10-25
First web release, based on the MultiHero Android port 1.0.1
([richardyusan/multihero-android](https://github.com/richardyusan/multihero-android)).
- MultiHero 0.96 builds to WebAssembly with Emscripten and runs in the browser (`build_web.sh`).
- Keyboard, mouse, gamepad (Gamepad API) and on-screen touch controls on phones/tablets.
- Sound with Web Audio; settings (`game.cfg`, `keys.cfg`) saved in the browser (IndexedDB).
- Original speed: game logic at 54 fps, display capped at 60 fps.
- Playable online with GitHub Pages (`docs/`); game data LZ4-compressed (25 MB → 11 MB download).
- `mh.cpp` is compiled without inlining so the ASYNCIFY link finishes in about a minute.
