# Changelog

## 1.0.0 – 2026-10-25
First web release, based on the MultiHero Android port 1.0.1
([richardyusan/multihero-android](https://github.com/richardyusan/multihero-android)).
- MultiHero 0.96 builds to WebAssembly with Emscripten and runs in the browser (`build_web.sh`).
- Keyboard, mouse, gamepad (Gamepad API) and on-screen touch controls on phones/tablets.
- Sound with Web Audio; settings (`game.cfg`, `keys.cfg`) saved in the browser (IndexedDB).
- Original speed: game logic at 54 fps, display capped at 60 fps.
- Playable online with GitHub Pages (`docs/`); game data LZ4-compressed (25 MB → 11 MB download).
- `mh.cpp` is compiled without inlining so the ASYNCIFY link finishes in about a minute.
