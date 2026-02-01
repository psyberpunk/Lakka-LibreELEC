# sx05re Package Collection

This directory contains packages imported from the [EmuELEC project](https://github.com/EmuELEC/EmuELEC), specifically from the `packages/sx05re` directory.

## Contents

### EmulationStation Frontend
- **emuelec-emulationstation** - Alternative frontend to RetroArch's XMB interface
  - Provides game collection browser with scraper integration
  - Supports themes and custom configurations
  - Source: https://github.com/EmuELEC/emuelec-emulationstation

### Libretro Cores

This directory contains additional libretro cores not present in the main Lakka emulation packages:

#### PlayStation Emulation
- **beetle-psx-hw** - Hardware-accelerated PlayStation emulator
- **duckstation-lr** - Fast PlayStation emulator (prebuilt binary)
- **swanstation** - PlayStation emulator fork

#### Nintendo 64 Emulation
- **mupen64plus-nx** - Modern N64 emulator
- **mupen64plus-nx-alt** - Alternative build of mupen64plus
- **parallel-n64** - Parallel RDP-based N64 emulator

#### Arcade Emulation
- **fbalpha** - Final Burn Alpha arcade emulator
- **fbalpha2012** - Final Burn Alpha 2012 version
- **mame** - Current MAME emulator

#### Sega Saturn
- **yabasanshiro** - Saturn emulator based on YabaSanshiro

#### Nintendo DS
- **desmume** - Nintendo DS emulator
- **melonds** - Modern Nintendo DS emulator

#### Dreamcast
- **flycast** - Dreamcast emulator (newer version)

#### Sega Genesis/Mega Drive
- **genesis-plus-gx** - Enhanced Genesis/Mega Drive emulator

#### Modern/Fantasy Consoles
- **tic-80** - Fantasy computer for making tiny games
- **wasm4** - WebAssembly-based fantasy console
- **lowresnx** - Fantasy console for retro-style games

#### Other Systems
- **easyrpg** - RPG Maker 2000/2003 game player
- **ppsspp** - PSP emulator (libretro core)

## Integration Notes

### Compatibility with Lakka
Most packages have been imported as-is from EmuELEC. Some packages may require adaptation:

1. **Device-specific configurations**: EmuELEC uses `DEVICE` variable for device-specific builds. These may need to be adapted to use Lakka's `PROJECT` variable.

2. **Dependencies**: Some packages depend on EmuELEC-specific components that may need to be substituted or added to Lakka.

3. **Build system**: Packages should be compatible with LibreELEC's build system, but may require testing.

### Usage

To enable these packages in your Lakka build, they need to be added to the appropriate distribution options file or project-specific package lists.

Example for adding to a build:
```bash
# Add to distributions/Lakka/options or project-specific configuration
ADDITIONAL_PACKAGES="$ADDITIONAL_PACKAGES duckstation-lr yabasanshiro tic-80"
```

## Credits

These packages are maintained by the EmuELEC team:
- Original source: https://github.com/EmuELEC/EmuELEC
- Primary maintainer: Shanti Gilbert (https://github.com/shantigilbert)
- EmuELEC community contributors

## License

Individual packages have their own licenses. See each package's `package.mk` file for license information.

Common licenses:
- GPL-2.0-or-later
- MIT
- NON-COMMERCIAL (some emulators)
- ISC
