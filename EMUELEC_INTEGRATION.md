# EmuELEC Integration into Lakka

## Overview

This document describes the integration of EmuELEC packages from https://github.com/EmuELEC/EmuELEC/tree/dev/packages/sx05re into the Lakka-LibreELEC project.

## Package Analysis

### Libretro Cores Comparison

**Lakka:** 75 libretro cores  
**EmuELEC:** 153 libretro cores  
**New cores to add:** 99 cores not present in Lakka  
**Common cores:** 54 cores (need version comparison)  
**Lakka-only cores:** 21 cores (retained)

### New Libretro Cores from EmuELEC (99 cores)

High-value additions include:
- **PlayStation Emulation:** duckstation-lr, swanstation, beetle-psx-hw
- **N64 Emulation:** mupen64plus, mupen64plus-nx, parallel-n64
- **Advanced Arcade:** mame, mame2015, mame2016, fbalpha, fbalpha2012
- **Dreamcast:** flycast (newer version)
- **Saturn:** yabasanshiro
- **Nintendo DS:** desmume, desmume-2015, melonds
- **PSP:** ppsspp (libretro core)
- **Game Boy/GBA:** gearboy, gearsystem, gearcoleco
- **Modern Systems:** easyrpg, tic-80, wasm4, lowresnx
- **PC Emulation:** dosbox-svn
- And 80+ more cores...

### Standalone Emulators (30+ packages)

EmuELEC includes standalone (non-libretro) emulators:
- **PPSSPPSDL** - PSP emulator
- **dolphinSA** - GameCube/Wii emulator
- **amiberry** - Amiga emulator
- **mupen64plussa** - N64 emulator
- **flycastsa** - Dreamcast emulator
- **duckstation** - PlayStation emulator
- **retroarch** - RetroArch (EmuELEC version)
- And 23+ more...

### EmulationStation Frontend

The `emuelec-emulationstation` package provides an alternative frontend to RetroArch's XMB interface:
- **Source:** https://github.com/EmuELEC/emuelec-emulationstation
- **Features:** Game collection browser, scraper integration, theme support
- **Dependencies:** SDL2, freetype, freeimage, vlc, rapidjson, SDL2_mixer, p7zip, espeak
- **Configuration:** es_systems.cfg, themes, scripts

## Integration Strategy

### Phase 1: Core Structure (COMPLETED)
- [x] Created `packages/sx05re/` directory
- [x] Copied EmulationStation package structure
- [x] Documented integration plan

### Phase 2: Package Import

#### 2.1 EmulationStation Frontend
- Import `emuelec-emulationstation` package
- Adapt device-specific configurations for Lakka targets
- Add dependencies (Crystal theme, etc.)

#### 2.2 New Libretro Cores (Priority List)
High-priority cores to add first:
1. **PlayStation:** duckstation-lr, swanstation, beetle-psx-hw
2. **N64:** mupen64plus-nx, parallel-n64
3. **Arcade:** mame, mame2015, mame2016
4. **DS:** desmume, melonds
5. **Saturn:** yabasanshiro
6. **Modern:** tic-80, wasm4, lowresnx
7. **Enhanced versions:** genesis-plus-gx, flycast

#### 2.3 Standalone Emulators (Optional)
Consider adding popular standalone emulators:
- dolphinSA (GameCube/Wii)
- PPSSPPSDL (PSP)
- duckstation (PlayStation)

#### 2.4 Ports (Optional)
EmuELEC includes 37+ game ports that could be added

### Phase 3: Version Conflict Resolution

For 54 common cores, compare versions and use the newer one:
- Review each core's version hash
- Check commit dates and features
- Update to best version
- Document version choices

### Phase 4: Build System Integration

#### 4.1 Distribution Configuration
Update `distributions/Lakka/options`:
```bash
# EmulationStation frontend support
EMULATIONSTATION_FRONTEND="no"  # Set to "yes" to enable

# sx05re additional cores
SX05RE_CORES="yes"
```

#### 4.2 Virtual Package
Create `packages/sx05re/sx05re-cores/package.mk`:
```makefile
PKG_NAME="sx05re-cores"
PKG_VERSION="1.0"
PKG_SECTION="virtual"
PKG_SHORTDESC="EmuELEC sx05re libretro cores collection"
PKG_DEPENDS_TARGET="toolchain \
  duckstation-lr \
  mupen64plus-nx \
  yabasanshiro \
  ... (all new cores)
"
```

### Phase 5: Testing & Validation
- Test builds for key platforms (Generic x86_64, RPi4, RPi5)
- Verify no package conflicts
- Test EmulationStation integration
- Validate core functionality

## Version Conflict Examples

| Core | Lakka Version | EmuELEC Version | Action |
|------|---------------|-----------------|--------|
| 2048 | 5474ed1 | 86e02d3 | Compare & update |
| beetle-lynx | 7fead71 | efd1797 | Compare & update |
| beetle-pce | af28fb0 | d5c2b28 | Compare & update |
| beetle-saturn | 0a78a9a | ccba526 | Compare & update |
| bluemsx | 572c918 | 3a2855e | Compare & update |
| cap32 | dbfa1aa | a5d96c5 | Compare & update |

## Implementation Notes

### Device-Specific Considerations
EmuELEC packages often include device-specific patches and configurations:
- **Rockchip devices:** OdroidGoAdvance, GameForce, RK356x
- **Amlogic devices:** Amlogic-old, Amlogic-ng, Amlogic-no
- **Generic x86_64**

For Lakka integration:
- Remove device-specific patches not applicable to Lakka targets
- Adapt configurations for Lakka's supported platforms
- Maintain compatibility with Lakka's build system

### Dependencies
Many EmuELEC packages depend on:
- **Crystal theme** - EmulationStation theme
- **espeak** - Text-to-speech
- **xmlstarlet** - XML processing
- **p7zip** - Archive support
- **vlc** - Video playback

These need to be added to Lakka or substituted with equivalents.

### Build System Differences
EmuELEC uses:
- `DEVICE` variable for device-specific builds
- Custom build flags and patches
- Different directory structures

Lakka uses:
- `PROJECT` variable for platform-specific builds
- LibreELEC build system conventions
- `packages/` category structure

Packages need adaptation to use Lakka's build conventions.

## Recommendations

### Minimal Integration Approach
For a minimal, focused integration:
1. **Add EmulationStation** as an optional frontend (can coexist with RetroArch)
2. **Import 20-30 high-value new cores** (not all 99)
3. **Update common cores** to newer versions where beneficial
4. **Skip standalone emulators** initially (focus on libretro)
5. **Skip ports** (not core functionality)

### Full Integration Approach
For a complete integration:
1. Import all 99 new libretro cores
2. Import all standalone emulators
3. Import game ports
4. Add EmulationStation as primary frontend option
5. Update all common cores to best versions
6. Add all dependencies and tools

## Current Status

- ✅ Repository structure analyzed
- ✅ Package comparison completed
- ✅ sx05re directory created
- ✅ EmulationStation package copied
- ⏳ Adaptation and testing needed
- ⏳ Version conflict resolution
- ⏳ Build system integration

## Next Steps

1. Decide on integration scope (minimal vs. full)
2. Adapt EmulationStation package for Lakka
3. Import priority libretro cores
4. Resolve version conflicts
5. Test builds
6. Document changes

## References

- **EmuELEC Repository:** https://github.com/EmuELEC/EmuELEC
- **sx05re Packages:** https://github.com/EmuELEC/EmuELEC/tree/dev/packages/sx05re
- **Lakka Build Docs:** BUILD.md
- **LibreELEC Build System:** https://github.com/LibreELEC/LibreELEC.tv
