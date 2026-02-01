# EmuELEC to Lakka Integration Guide

## Summary

This guide documents the integration of EmuELEC packages (from https://github.com/EmuELEC/EmuELEC/tree/dev/packages/sx05re) into the Lakka-LibreELEC project.

## What Was Integrated

### 1. EmulationStation Frontend
- **Package:** `emuelec-emulationstation`
- **Location:** `packages/sx05re/emuelec-emulationstation/`
- **Purpose:** Alternative frontend to RetroArch's XMB interface
- **Features:**
  - Game collection browser with metadata
  - Scraper integration for game information
  - Theme support (Crystal theme included)
  - System-specific configurations
  - Text-to-speech support

### 2. Libretro Cores (41 Additional Cores)

#### High-Impact Additions

**Modern Console Emulation:**
- `duckstation-lr` - Fast PlayStation emulator (prebuilt)
- `swanstation` - PlayStation emulator fork
- `beetle-psx-hw` - Hardware-accelerated PlayStation
- `lr-dolphin` - GameCube/Wii emulator (libretro)
- `ppsspp` - PSP emulator (libretro core)
- `yabasanshiro` - Sega Saturn emulator

**Enhanced N64 Emulation:**
- `mupen64plus-nx` - Modern N64 emulator
- `mupen64plus-nx-alt` - Alternative build
- `parallel-n64` - Parallel RDP-based emulator

**Nintendo DS Emulation:**
- `desmume` - Nintendo DS emulator
- `melonds` - Modern DS emulator with better accuracy

**Advanced Arcade:**
- `mame` - Current MAME (full set)
- `mame2015` - MAME 2015 version
- `mame2016` - MAME 2016 version
- `mame2003-plus` - Enhanced MAME 2003
- `fbalpha` - Final Burn Alpha
- `fbalpha2012` - FBA 2012 version

**Enhanced System Emulation:**
- `flycast` - Dreamcast (newer version)
- `genesis-plus-gx` - Enhanced Genesis/Mega Drive
- `neocd_libretro` - Neo Geo CD

**Retro Computer Systems:**
- `np2kai` - NEC PC-98 emulator
- `px68k` - Sharp X68000 emulator
- `quasi88` - NEC PC-8801 emulator
- `puae` - Amiga emulator
- `puae2021` - Updated Amiga emulator

**Game Boy Family:**
- `gearboy` - Game Boy/Game Boy Color
- `gearsystem` - Sega Master System/Game Gear
- `gearcoleco` - ColecoVision
- `gpsp` - Game Boy Advance
- `vba-next` - Enhanced GBA emulator

**Fantasy/Modern Consoles:**
- `tic-80` - Fantasy computer for making tiny games
- `wasm4` - WebAssembly-based fantasy console
- `lowresnx` - Retro-style fantasy console
- `vircon32` - 32-bit fantasy console
- `fake_08` - PICO-8 fantasy console emulator
- `craft` - Minecraft-inspired game

**Classic Systems:**
- `freechaf` - Fairchild Channel F
- `freeintv` - Intellivision
- `theodore` - Thomson TO8/TO9 computers

**Other:**
- `easyrpg` - RPG Maker 2000/2003 player
- `dosbox-svn` - DOSBox (newer SVN version)

### 3. Standalone Emulators (5 High-Performance Emulators)

**Location:** `packages/sx05re/emulators/`

- **PPSSPPSDL** - PSP emulator with SDL backend
  - Optimized for performance
  - Full cheat support
  - Custom controller configurations
  
- **dolphinSA** - GameCube/Wii emulator
  - Standalone version for better performance
  - Custom hotkeys and controller mapping
  
- **duckstation** - PlayStation emulator
  - High accuracy and performance
  - Achievement support
  - Advanced rendering options
  
- **flycastsa** - Dreamcast emulator
  - Standalone version
  - Achievement support
  - Custom controller configurations
  
- **amiberry** - Amiga emulator
  - Optimized for ARM platforms
  - Pre-configured for A500 and A1200
  - Extensive hardware emulation

## Package Structure

```
packages/sx05re/
├── README.md                     # Documentation
├── emuelec-emulationstation/     # Frontend package
│   ├── package.mk
│   ├── config/                   # ES configuration files
│   ├── themes/                   # Theme packages
│   └── patches/                  # Platform-specific patches
├── libretro/                     # Libretro cores (41 cores)
│   ├── duckstation-lr/
│   ├── yabasanshiro/
│   ├── mupen64plus-nx/
│   ├── mame/
│   └── ... (37 more cores)
└── emulators/                    # Standalone emulators (5 emulators)
    ├── PPSSPPSDL/
    ├── dolphinSA/
    ├── duckstation/
    ├── flycastsa/
    └── amiberry/
```

## Key Differences from Original EmuELEC

### Retained from EmuELEC:
- All package.mk files (build definitions)
- Configuration files for emulators
- Patches for platform-specific fixes
- Scripts for launching emulators
- Theme packages for EmulationStation

### Adaptation Needed:
1. **Device Variables:** EmuELEC uses `DEVICE` (e.g., "OdroidGoAdvance", "Amlogic-old"). Lakka uses `PROJECT` (e.g., "RPi4", "Generic").
2. **Dependencies:** Some packages depend on EmuELEC-specific components that may need adaptation.
3. **Build Integration:** Packages need to be added to Lakka's distribution configuration.

## Version Conflicts

### Common Cores with Different Versions (20 cores)

The following cores exist in both Lakka and EmuELEC but with different versions (git commits):

| Core | Status | Recommendation |
|------|--------|----------------|
| 2048 | Different git hash | Compare features, use newer |
| beetle-lynx | Different git hash | Compare features, use newer |
| beetle-pce | Different git hash | Compare features, use newer |
| beetle-saturn | Different git hash | Compare features, use newer |
| bluemsx | Different git hash | Compare features, use newer |
| cap32 | Different git hash | Compare features, use newer |
| gambatte | Different git hash | Compare features, use newer |
| handy | Different git hash | Compare features, use newer |
| mrboom | Different git hash | Compare features, use newer |
| nestopia | Different git hash | Compare features, use newer |
| pokemini | Different git hash | Compare features, use newer |
| prboom | Different git hash | Compare features, use newer |
| scummvm | Different git hash | Compare features, use newer |
| snes9x | Different git hash | Compare features, use newer |
| stella | Different git hash | Compare features, use newer |
| ... | ... | ... |

**Action Required:** For each conflicting core, determine which version is newer/better and update accordingly.

## Integration Status

### ✅ Completed
- [x] Repository structure analysis
- [x] Package comparison and identification
- [x] Created `packages/sx05re/` directory
- [x] Imported EmulationStation package
- [x] Imported 41 high-priority libretro cores
- [x] Imported 5 standalone emulators
- [x] Created integration documentation

### ⏳ Pending
- [ ] Adapt device-specific configurations for Lakka platforms
- [ ] Create build system integration (distribution options)
- [ ] Test core package builds
- [ ] Resolve version conflicts with existing cores
- [ ] Test EmulationStation frontend integration
- [ ] Create user documentation
- [ ] Test on target hardware (RPi4, RPi5, Generic x86_64)

## Usage Instructions (Future)

### For Developers

To enable sx05re packages in a Lakka build:

1. **Add to distribution options** (`distributions/Lakka/options`):
   ```bash
   # Enable sx05re cores
   SX05RE_CORES="yes"
   
   # Enable EmulationStation frontend (optional)
   EMULATIONSTATION="no"  # Set to "yes" to enable
   ```

2. **Build specific cores:**
   ```bash
   PROJECT=RPi4 ARCH=aarch64 scripts/build duckstation-lr
   PROJECT=RPi4 ARCH=aarch64 scripts/build yabasanshiro
   ```

3. **Build EmulationStation:**
   ```bash
   PROJECT=RPi4 ARCH=aarch64 scripts/build emuelec-emulationstation
   ```

### For Users

Once integrated and tested, users will be able to:

1. **Use new emulator cores** in RetroArch
2. **Switch to EmulationStation frontend** (optional)
3. **Access enhanced emulation** for PlayStation, N64, DS, Saturn, and more
4. **Use standalone emulators** for better performance on specific systems

## Testing Plan

### Phase 1: Build Testing
1. Test building individual cores on Generic x86_64
2. Test building cores on RPi4 (ARM)
3. Verify no build conflicts with existing packages

### Phase 2: Functionality Testing
1. Test core loading in RetroArch
2. Test ROM compatibility
3. Verify configurations work correctly

### Phase 3: EmulationStation Testing
1. Test EmulationStation build
2. Test frontend integration
3. Test theme support
4. Verify system detection and ROM scanning

### Phase 4: Platform Testing
1. Test on Raspberry Pi 4
2. Test on Raspberry Pi 5
3. Test on Generic x86_64
4. Test on other supported platforms

## Known Issues

### Potential Challenges

1. **Device-Specific Code:** Many packages have device-specific patches for EmuELEC devices (OdroidGoAdvance, Amlogic SoCs). These need to be adapted for Lakka's platforms.

2. **Dependencies:** Some packages depend on:
   - `espeak` (text-to-speech)
   - `xmlstarlet` (XML processing)
   - `p7zip` (archive support)
   - `vlc` (video playback)
   
   These may need to be added to Lakka or substituted.

3. **Binary Packages:** `duckstation-lr` is distributed as a prebuilt binary for aarch64. This may not work on all architectures.

4. **Build System Differences:** EmuELEC's build variables (`DEVICE`, `OPENGLES`) may need translation to Lakka's equivalents.

## Maintenance

### Keeping Up-to-Date

To update sx05re packages from EmuELEC in the future:

1. Monitor EmuELEC repository for updates
2. Check package version changes
3. Update individual packages as needed
4. Test updated packages before committing

### Contributing Back

Consider contributing improvements back to EmuELEC:
- Bug fixes
- Build system improvements
- Platform compatibility patches

## Credits

- **EmuELEC Team:** Original packages and integration work
  - Lead: Shanti Gilbert (https://github.com/shantigilbert)
  - Repository: https://github.com/EmuELEC/EmuELEC

- **Lakka Team:** Build system and distribution
  - Repository: https://github.com/libretro/Lakka-LibreELEC

- **LibreELEC:** Build system framework
  - Repository: https://github.com/LibreELEC/LibreELEC.tv

## License

Individual packages have their own licenses. Common licenses include:
- GPL-2.0-or-later
- MIT
- ISC
- NON-COMMERCIAL (some emulators)

See individual `package.mk` files for specific license information.

## Support

For questions or issues:
1. Check the integration documentation in this directory
2. Review individual package.mk files
3. Consult Lakka forums: https://forums.libretro.com/c/libretro/lakka-tv-general
4. Reference EmuELEC documentation: https://github.com/EmuELEC/EmuELEC

---

**Integration Date:** February 2026  
**EmuELEC Source Version:** dev branch (as of Feb 2026)  
**Status:** Initial integration complete, testing pending
