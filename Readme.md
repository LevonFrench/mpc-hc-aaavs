# mpc-hc-aaavs

An independent fork of [MPC-HC](https://github.com/clsid2/mpc-hc) that embeds the [AAAVS](https://github.com/LevonFrench/aaavs) music visualizer in the album-art area. MPC-HC handles decoding and playback; the visualizer receives decoded audio and follows the player's song clock. Video playback keeps the usual video renderer.

**Status: parked.** This is preview 0.1.0 of the fork, for Windows x64, and is not under active development. `version.txt` still carries the upstream MPC-HC base version, not the fork's preview number. It is not an official MPC-HC release and is not affiliated with the MPC-HC project.

The fork is based on upstream MPC-HC (clsid2/mpc-hc) commit `8e1cc8761f8dfb0352779ace61c85774e4705fd5`. This repository's history starts with the fork's first public commit; upstream history is at the link above. The changes this fork makes are listed in [docs/AAAVS-CHANGES.md](docs/AAAVS-CHANGES.md). The original MPC-HC README is kept as [docs/MPC-HC-UPSTREAM-README.md](docs/MPC-HC-UPSTREAM-README.md).

## Getting the source

The visualizer lives in its own repository and is included here as the `visualizer` git submodule. The third-party libraries under `src/thirdparty` are git submodules as well, pinned to the commits listed in [docs/THIRDPARTY-SUBMODULES.txt](docs/THIRDPARTY-SUBMODULES.txt). Clone with submodules:

```
git clone --recurse-submodules https://github.com/LevonFrench/mpc-hc-aaavs.git
```

If you already cloned without them, run `git submodule update --init --recursive`. The `visualizer` submodule points at the [AAAVS repository](https://github.com/LevonFrench/aaavs), so the clone succeeds only while that repository is public.

## What a build needs

The current build is Release Lite: it requires compatible installed DirectShow codecs, the Microsoft Edge WebView2 Runtime, and the DirectX End-User Runtime. It is not a self-contained codec bundle. Automatic update checks are disabled; the player does not offer upstream MPC-HC builds as fork updates.

Native builds, CPU regression checks, live playback and GPU validation are separate steps. Follow the [build guide](docs/BUILDING.md) (Visual Studio 2022 C++ tools, Python 3, Node.js 22.18 or newer) and the upstream [compilation notes](docs/Compilation.md) for the native toolchain. In short, from the checkout root:

```
powershell -ExecutionPolicy Bypass -File tools/prepare-aaavs.ps1
python tools/check-native-audio.py
python tools/build-native.py
powershell -ExecutionPolicy Bypass -File tools/stage-aaavs.ps1
start-mpc-hc-aaavs.cmd
```

`build-native.py` produces `bin/mpc-hc_x64 Lite/mpc-hc-aaavs.exe`; keep the staged `visualizer` folder beside the executable. Enable MPC-HC's AudioSwitcher for visualizer audio, since bitstream passthrough does not expose decoded PCM.

## Using the visualizer

The toolbar adds previous preset, next preset, shuffle, and Auto beside the playback controls. **View > Visualizer** opens the manager, setup builder, and visualizer options. Playback transport keeps its ordinary function.

| Default shortcut | Action |
| --- | --- |
| Ctrl+F6 | Open Preset Manager |
| Ctrl+F7 | Open Setup Builder |
| F6 / F7 | Lower / raise the displayed preset's rating |
| F8 | Mark the displayed preset as not working |
| Escape | Close the management panel |

All visualizer commands are listed with MPC-HC commands in **Options > Player > Keys**. The visualizer's preset, transition and scene documentation is in the `visualizer` submodule: `visualizer/docs/PRESET-MANAGEMENT.md`, `visualizer/docs/AVS-TRANSITIONS.md` and `visualizer/docs/NERV-SCENES.md`. See also the [audio implementation notes](docs/AUDIO-WIRING.md).

Historical AVS preset packs, bitmap packs, fonts and APE binaries are not distributed. A compatible catalog placed under `visualizer/avs presets/` beside the executable is verified by size and SHA-256 before rendering, and historical APE binaries are never executed. Preset and dependency packs have their own terms.

## Release

The [release guide](docs/RELEASING.md) describes the packager, which builds a public-only archive with a SHA-256 inventory. The [preview release notes](docs/RELEASE-NOTES.md) record the scope and what still needs live validation.

## License and attribution

This fork builds on upstream MPC-HC commit `8e1cc8761f8dfb0352779ace61c85774e4705fd5` and is distributed under the [GNU GPL v3](COPYING.txt), with the existing third-party notices retained. See also the [AVS transition notice](THIRD-PARTY-AVS-TRANSITIONS.txt) and the [NERV attribution](THIRD-PARTY-NERV.txt). MPC-HC authors are listed in [docs/Authors.txt](docs/Authors.txt).
