<h1 align=center>The King of Fighters 2002 Unlimited Match — Nintendo Switch port</h1>

**English** | [Español](README.es.md)

A native Nintendo Switch port of the Xbox 360 (XBLA) version of **The King of Fighters 2002 Unlimited Match**.

It is not an emulator. The PowerPC code in the game's `default.xex` is statically recompiled to C++ with
[ReXGlue](https://github.com/rexglue/rexglue-sdk), built for the Switch with devkitPro/libnx, and the game is
rendered by a Vulkan backend running on NVK (Mesa).

> [!NOTE]
> This repository **contains no game data and no code derived from the game**. You need your own copy of the
> Xbox 360 game (the XBLA package). The [installer page](https://tbittech.github.io/kof2002um-nx-installer/)
> converts it into the files the Switch needs.

## Status

Work in progress. In handheld mode, at the console's stock clocks with no overclock (CPU 1020 MHz, GPU 460 MHz,
memory 1331 MHz), a full fight runs at ~58 FPS (up from ~45; the target is a stable 60). Below 60 the game runs a
little slower than normal, because its logic advances one step per drawn frame.

**Controllers:** L+R+ZR opens the Switch controller applet (up to 4 players, detached Joy-Con or Pro Controller).
Two-player play has not been tested yet.

**Known issues:** dock mode has not been tested with the 11/11 improvements yet (the previous dock measurement,
from 11/07, was ~52–56 FPS).

**Fixed (2026-11-07):** the character select screen was black (no grid, background or cursors), and the team order
screen was missing its background and UI. Both are drawn correctly now, with no cost to fight performance.

The detailed log is in [PORT_STATUS.md](PORT_STATUS.md) (Spanish, newest first).

## Installing on the Switch

The easiest way is the **installer page: <https://tbittech.github.io/kof2002um-nx-installer/>**. Give it your XBLA
package (or the folder extracted from it) and it builds `kof2002um-nx.zip` with the NRO, the `.toml` and the game
files, entirely inside your browser: your copy is never uploaded anywhere. Unzip it into `sdmc:/switch/` and you get:

```text
/switch/kof2002um-nx/
  kof2002um-nx.nro
  kof2002um_nx.toml
  game.nxpack + game.nxidx   (the game files, built from your copy)
```

Launch it in full application mode (hold **R** while opening an installed title, or use a forwarder). Applet mode
(Album) does not have enough memory.

## Layout

| Path | What it is |
|---|---|
| `src/recompiler/` | ReXGlue manifest, CMake project and the port's own code (hooks, autotest/bench support) |
| `tools/rexglue-sdk/` | ReXGlue SDK with the Switch runtime: Horizon layer, Vulkan/NVK renderer changes, performance counters |
| `tools/fetch_thirdparty.py` | Downloads the SDK dependencies at their pinned commits (`thirdparty_pins.txt`) |
| `tools/kof_stfs_extract.py` | Extracts the XBLA STFS package, even with a missing block (locates blocks by SHA-1) |
| `tools/xctd/` | Decompresses the game's XCTD (LZX) files |
| `tools/ghidra-scripts/` | Headless Ghidra scripts used for reverse engineering |
| `switch/kof2002um_nx.toml` | Runtime configuration that goes next to the NRO (the current 60 FPS–oriented set) |
| `bench/devkitnx-configs/` | Configurations for the automated on-console benchmark (devkitnx) |

## Building (summary)

1. Install devkitPro (devkitA64 + libnx) and download the SDK dependencies: `python tools/fetch_thirdparty.py`.
2. Extract your XBLA package: `python -I tools/kof_stfs_extract.py <package> game/`, then decompress the XCTD
   files with `tools/xctd/`.
3. Run the ReXGlue codegen on your `game/default.xex` with `src/recompiler/kof2002um_nx_manifest.toml`
   (the output goes to `src/recompiler/generated/`, which is never committed).
4. Configure and build `src/recompiler` with the Switch preset from `CMakePresets.json`.

## Credits

- [ReXGlue](https://github.com/rexglue/rexglue-sdk) and the [Xenia](https://github.com/xenia-project/xenia)
  project, whose GPU and kernel code the runtime derives from.
- [nfsmw-nx](https://github.com/StevensND/nfsmw-nx), another ReXGlue port to the Switch.
- devkitPro, libnx and Mesa/NVK.

## Support

If you like my work and want to support me:

<a href="https://tecito.app/tebit"><img src="https://img.shields.io/badge/Buy_me_a-tecito-6d4aff?style=for-the-badge" alt="Buy me a tecito" height="32"></a>
