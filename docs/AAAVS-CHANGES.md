# What mpc-hc-aaavs changes relative to MPC-HC

Upstream: [clsid2/mpc-hc](https://github.com/clsid2/mpc-hc), base commit `8e1cc8761f8dfb0352779ace61c85774e4705fd5`. The fork is licensed under the GNU GPL v3 ([COPYING.txt](../COPYING.txt)), like upstream.

## New files

All under `src/mpc-hc/`:

- `AAAVSView.h`, `AAAVSView.cpp`: hosts the visualizer in a WebView2 control inside the player's album-art area, feeds it decoded audio and the player's song clock, and handles preset, setup and rating commands.
- `AAAVSLibrary.h`: header-only preset-library code (path validation, rating renames with rollback, persistent status and undo, page state storage).
- `AAAVSTransitionNames.h`: names of the AVS-style transition styles shown in the options.

## Changed upstream files

The visualizer is wired in through small hooks in these files:

- `MainFrm.cpp`, `MainFrm.h`: creates and drives the visualizer view, audio handoff, commands, shortcuts and timing.
- `ChildView.cpp`, `ChildView.h`: ticks, resizes and closes the visualizer view in the album-art area.
- `MouseTouch.cpp`: lets an open visualizer management panel take pointer input.
- `AppSettings.cpp`, `mplayerc.cpp`, `mplayerc.h`, `Profile.cpp`: visualizer default key bindings, and the renamed product, log prefix and separate settings location.
- `PlayerToolBar.cpp`, `resource.h`, `mpc-hc.rc`: the preset, rating, shuffle and Auto toolbar buttons and the visualizer commands.
- `AboutDlg.cpp`, `UpdateChecker.cpp`: product name, and the update check is replaced by a message that explains how to get fork builds.
- `mpc-hc.vcxproj`: new source files, product name `mpc-hc-aaavs`, and the WebView2 SDK (restored by `tools/prepare-aaavs.ps1`).

## Other changes

- `visualizer`: git submodule of the AAAVS repository, the web visualizer that the player embeds.
- `tools/`: native audio and preset-library checks (`check-native-audio.py`, `check-aaavs-audio.cpp`, `check-aaavs-library.cpp`), the native builder (`build-native.py`), SDK preparation and staging scripts, and the release packager and its check.
- `docs/`: BUILDING, RELEASING, RELEASE-NOTES, AUDIO-WIRING and this file.
- `THIRD-PARTY-AVS-TRANSITIONS.txt`, `THIRD-PARTY-NERV.txt`: attribution for the transition styles and the NERV-style scene set.
- `src/thirdparty`: third-party libraries are git submodules at the commits in [THIRDPARTY-SUBMODULES.txt](THIRDPARTY-SUBMODULES.txt), not vendored copies.

- `distrib/`: the binaries tracked there (`crashrpt.dll`, `D3DCompiler_47.dll`, `D3DX9_43.dll`, `dbghelp.dll` and `sendrpt.exe`, in `x64` and `x86`) are inherited unchanged from upstream MPC-HC.

Everything else is upstream MPC-HC code and keeps its original copyright and licence headers.

