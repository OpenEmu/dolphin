# Dolphin - A GameCube / Wii core that works on OpenEmu


NOTE:   This is not a core supported by the OpenEmu Team.  Issues will not be handled by the OpenEmu team.
        Most issues such as sound and graphics are rooted in the dolphin emulator and not OpenEmu.
        This is a Working-As-Is core.
        The un-official fork of OpenEmu with Wii Support is not supported by the OpenEmu Team and issues with
        the this fork will not be handled by the OpenEmu Team.

NOTE:   This is not a core supported by the Dolphin Team.  Issues will not be handled by the Dolphin team.
                This is a Working-As-Is core.



         Do not contact the OpenEmu IRC or Dolphin IRC with questions about this core or the Open-Emu fork here.


Dolphin-Core is GameCube and Wii emulation core for OpenEmu.

It is a Work in Progress and is already running many GameCube games in the latest OpenEmu compiled from git source.

Wii support is added with the git source fork of Open-Emu and OpenEmu-SDK from https://github.com/duckey77.

Please read the [FAQ](https://dolphin-emu.org/docs/faq/) before using Dolphin.

## System Requirements
### Desktop

    * OpenEmu compiled from git source
    * OS X (10.11 higher).

### Dolphin-Core supports the following file formats
Wii:
  wad
  ciso
  iso

GC:
  gc
  gcz
  ciso

## Building the core for Apple Silicon (arm64)

The project does not set up every include directory and source file that the
pinned dolphin revision needs, so the build takes its settings from
`openemu-arm64.xcconfig` (and `openemu-arm64-release.xcconfig`, which adds
`NDEBUG`). Two things have to exist before building:

1. An arm64 `OpenEmuBase.framework` build from the OpenEmu workspace. Point
   `OE_OPENEMU_BUILD_DIR` at its `Build/Products/Debug` directory if it is not
   in the default location.
2. glslang's `build_info.h`, which upstream generates with CMake. Generate it
   into the ignored build directory:

   ```
   python3 dolphin/Externals/glslang/glslang/build_info.py \
       dolphin/Externals/glslang/glslang \
       -i dolphin/Externals/glslang/glslang/build_info.h.tmpl \
       -o build/gen/glslang/build_info.h
   ```

Then build the release configuration. Debug works as well but is far too slow
for emulation, it is only useful for diagnostics:

```
xcodebuild -project Dolphin.xcodeproj -target Dolphin -configuration Release \
    ARCHS=arm64 ONLY_ACTIVE_ARCH=YES \
    -xcconfig openemu-arm64-release.xcconfig \
    CODE_SIGNING_ALLOWED=NO CONFIGURATION_BUILD_DIR=<output-dir> build
```

The resulting `Dolphin.oecoreplugin` has to be ad-hoc signed before OpenEmu can
load it (`codesign --force --deep -s - Dolphin.oecoreplugin`) and belongs in
`~/Library/Application Support/OpenEmu/Cores/`.

