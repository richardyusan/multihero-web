# Notice – what is covered by which terms

| Part | Owner / terms |
|---|---|
| `tools/`, `runtime/`, `web/`, `scripts/`, `build_web.sh`, `.github/`, `docs/` | Web port – MIT License (see `LICENSE`) |
| `src/*.bb` | Original MultiHero 0.96 source code by **mhman**, the creator of MultiHero, released as the "0.96 SHARED" source package. Included unchanged; it remains under its author's terms. |
| Game data (`gfx/`, `sounds/`, `Maps/`, `*.cfg`, `maps.dat`) | Not included as files. A packed copy is inside `docs/multihero.data`, only so the game can be played online (GitHub Pages). It remains the property of mhman and of the owners listed below. |

## Third-party characters

MultiHero is a non-commercial fan game. Many of its characters, sprites and sounds come from games and
franchises owned by third parties (for example Capcom, Nintendo, Rare, Marvel, DC, Konami, Koei Tecmo and others). Those
names and assets belong to their respective owners; this project claims no rights to them and is not
affiliated with or endorsed by any of them.

The online version is a free, non-commercial fan project. If you are a rights holder and want
something removed, please open an issue and it will be taken down.

## Tools used to build

- Emscripten SDK (MIT / University of Illinois NCSA licenses) – used at build time; its JavaScript
  loader is generated into `multihero.js`.
- The built-in debug font in `runtime/font8x16.h` is rendered from DejaVu Sans Mono
  (Bitstream Vera / DejaVu font license, free to use and embed).
