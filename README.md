# VMPK Extended

A personal fork of [Virtual MIDI Piano Keyboard](https://vmpk.sourceforge.io/) with additional playing controls and a macOS QWERTY keyboard map. The macOS application is called **VMPK.app**.

VMPK sends MIDI notes to a synthesizer. On macOS, the DLS Synth backend can provide sound using Apple's built-in sound set.

## Additions

### Optional pitch-bend hold

The **Hold** checkbox beside the Bender controls what happens when you release the slider:

- **Checked:** retain the selected bend. Enabled by default.
- **Unchecked:** return to centre. Unchecking also immediately clears a held bend.
- **Reset:** return to centre in either mode.

This can help match recordings that sit between standard pitches. The audible bend range depends on the synthesizer. Hold is currently a session control; its state and bend amount are not saved across restarts.

Show the Pitch Bender toolbar from the **View** menu if it is hidden.

### macOS QWERTY keyboard map

[`data/macos-qwerty-2.75-octaves.xml`](data/macos-qwerty-2.75-octaves.xml) maps **34 unique keys** to a continuous range of **33 semitones (2.75 octaves)**, from C through A.

| Range | White-note keys | Black-note keys |
| --- | --- | --- |
| Lower octave, C–B | Z X C V B N M | S D G H J |
| Upper range, C–A | Q W E R T Y U I O P [ ] &#92; | 2 3 5 6 7 9 0 - = |

Load the XML through **Edit → Keyboard Map → Load**, using the normal keyboard map rather than the raw map. Use English QWERTY without Shift. For a compact display, set **Preferences → Visualization → Number of keys** to **34** and **Starting Key** to **C**. Base Octave selects the playing register.

The XML is included in the macOS app resources and the installed data files.

## Settings backup and restore

Use VMPK's original **File → Save Configuration…** and **File → Load Configuration…** actions. Loading a configuration restarts VMPK. Referenced keyboard maps and sound files must remain available at their stored paths; configuration export does not bundle those files.

## Build from source

Requirements:

- C++17 compiler and CMake 3.16 or newer.
- Qt 6.2 or newer, including Widgets, Network, and LinguistTools; Qt 5.9 or newer is also supported with `-DUSE_QT5=ON`.
- [Drumstick](https://github.com/pedrolcl/drumstick) 2.11 or newer, including RT and Widgets.
- On macOS, Apple's command-line development tools.

```sh
git clone https://github.com/bchkv/vmpk-extended.git
cd vmpk-extended
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release -DBUILD_DOCS=OFF
cmake --build build --parallel
```

If Qt or Drumstick is installed outside CMake's search paths, pass their installation prefixes with `-DCMAKE_PREFIX_PATH="/path/to/drumstick;/path/to/qt"`.

For Homebrew Qt, `qttools` provides LinguistTools. Drumstick was built separately from its developer's GitHub mirror for the local macOS build.

On Apple Silicon, if compilation fails in Qt's `qyieldcpu.h` with an undeclared `__yield`, the local build was completed by adding `-DCMAKE_CXX_FLAGS="-include arm_acle.h"` when configuring CMake.

On macOS, the build produces `build/src/vmpk.app`. It can be copied as **VMPK.app**, but a development build can depend on libraries outside the bundle. Package Qt, Drumstick, and the MIDI backend plugins before distributing a standalone app. No standalone release is provided by this README.

## Status

The Hold checkbox and custom keyboard map are implemented. The modified application has been built on Apple Silicon with Qt 6.12 and Drumstick 2.11.2. Trackpad pitch-bend gestures are an idea for future work and are not implemented.

## Upstream and license

Based on VMPK by Pedro López-Cabanillas and its contributors:

- [Upstream website](https://vmpk.sourceforge.io/)
- [SourceForge repository](https://sourceforge.net/p/vmpk/git/)
- [GitHub mirror](https://github.com/pedrolcl/VMPK)

Distributed under **GPL-3.0-or-later**. See [COPYING](COPYING) and [AUTHORS](AUTHORS) for license terms and credits. Upstream downloads contain the original application; this fork's additions require building this repository.
