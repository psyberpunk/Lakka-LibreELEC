# Building Lakka from Source

This guide provides comprehensive instructions for compiling Lakka for all supported chipsets, including Raspberry Pi 5.

## Table of Contents

- [System Requirements](#system-requirements)
- [Supported Platforms](#supported-platforms)
- [Installation of Dependencies](#installation-of-dependencies)
- [Getting the Source Code](#getting-the-source-code)
- [Building Lakka](#building-lakka)
- [Building for Specific Platforms](#building-for-specific-platforms)
- [Building All Platforms](#building-all-platforms)
- [Using Docker](#using-docker)
- [Troubleshooting](#troubleshooting)

## System Requirements

### Recommended System Specifications

- **CPU**: Multi-core processor (4+ cores recommended)
- **RAM**: 8 GB minimum, 16 GB or more recommended
- **Disk Space**: 50-100 GB free space (varies by platform)
- **OS**: Ubuntu LTS (20.04, 22.04, or 24.04), Debian 12, or similar Linux distribution

### Build Time Estimates

Build times vary significantly based on your hardware and the target platform:
- **Single platform**: 1-4 hours on modern hardware
- **All platforms**: 24-48 hours or more

## Supported Platforms

Lakka supports building for the following chipsets and devices:

### ARM-based Platforms

#### Allwinner
- **A64** (aarch64)
- **H2-plus** (arm)
- **H3** (arm)
- **H5** (aarch64)
- **H6** (aarch64)
- **H616** (aarch64)
- **R40** (arm)

#### Amlogic
- **AMLGX** (aarch64)

#### Ayn
- **Odin** (aarch64)

#### NXP
- **iMX6** (arm)
- **iMX8** (aarch64)

#### Raspberry Pi
- **RPi** (Raspberry Pi 1) (arm)
- **RPi2** (Raspberry Pi 2) (arm)
- **RPi3** (Raspberry Pi 3) (aarch64)
- **RPi3-Composite** (aarch64)
- **RPi4** (Raspberry Pi 4) (aarch64)
- **RPi4-Composite** (aarch64)
- **RPi4-GPiCase2** (aarch64)
- **RPi4-PiBoyDmg** (aarch64)
- **RPi4-RetroDreamer** (aarch64)
- **RPi5** (Raspberry Pi 5) (aarch64) ⭐
- **RPi5-Composite** (aarch64)
- **RPiZero-GPiCase** (arm)
- **RPiZero2-GPiCase** (arm)
- **RPiZero2-GPiCase2W** (aarch64)

#### Rockchip
- **RK3288** (arm)
- **RK3328** (aarch64)
- **RK3399** (aarch64)

#### Samsung
- **Exynos** (arm)

### x86-based Platforms

#### Generic (PC)
- **Generic** (i386)
- **Generic** (x86_64)
- **Wayland** (x86_64)
- **X11** (x86_64)

### Other Platforms

#### L4T (NVIDIA Tegra)
- **Switch** (Nintendo Switch) (aarch64)

## Installation of Dependencies

### Ubuntu / Debian

On Ubuntu LTS (20.04, 22.04, 24.04) or Debian 12, the build system will automatically detect and prompt you to install missing dependencies. However, you can manually install the core dependencies:

```bash
sudo apt update
sudo apt install -y \
  bash bc bzip2 curl diffutils gawk gcc gperf gzip file \
  lsdiff lzop make patch perl rdfind rsync sed tar unzip \
  wget xz-utils zip zstd \
  g++ xsltproc default-jre python3 \
  libncurses5-dev libc6-dev \
  libjson-perl libparse-yapp-perl libxml-parser-perl \
  git
```

### Fedora / CentOS / RHEL

```bash
sudo dnf install -y \
  bash bc bzip2 curl diffutils gawk gcc gperf gzip file \
  patchutils lzop make patch perl rdfind rsync sed tar unzip \
  wget xz zip zstd \
  gcc-c++ xorg-x11-font-utils libxslt java-1.8.0-openjdk python3 \
  rpcgen glibc-static libstdc++-static ncurses-devel glibc-headers \
  perl-JSON perl-Parse-Yapp perl-Thread-Queue perl-XML-Parser \
  git
```

### Arch Linux

```bash
sudo pacman -S --needed \
  bash bc bzip2 curl diffutils gawk gcc gperf gzip file \
  patchutils lzop make patch perl rdfind rsync sed tar unzip \
  wget xz zip zstd \
  gcc xorg-mkfontscale xorg-mkfontdir xorg-bdftopcf libxslt \
  jdk8-openjdk python \
  perl-json perl-xml-parser perl-parse-yapp \
  git rpcsvc-proto
```

### openSUSE

```bash
sudo zypper install -y \
  bash bc bzip2 curl diffutils gawk gcc gperf gzip file \
  patchutils lzop make patch perl rdfind rsync sed tar unzip \
  wget xz zip zstd \
  gcc-c++ mkfontscale mkfontdir bdftopcf libxslt-tools \
  java-1_8_0-openjdk python3 \
  glibc-devel-static \
  git
```

### Gentoo / Sabayon

```bash
sudo emerge -av \
  bash bc bzip2 curl diffutils gawk gcc gperf gzip file \
  patchutils lzop make patch perl rdfind rsync sed tar unzip \
  wget xz-utils zip zstd \
  "gcc[cxx]" mkfontscale bdftopcf libxslt virtual/jre python \
  dev-perl/JSON dev-perl/Parse-Yapp dev-perl/XML-Parser \
  git
```

## Getting the Source Code

### Clone the Repository

For development and contributions, clone the official Lakka repository:

```bash
git clone https://github.com/libretro/Lakka-LibreELEC.git
cd Lakka-LibreELEC
```

### Switch to Development Branch

The development branch is `devel`:

```bash
git fetch origin devel:devel
git checkout devel
```

For stable releases, checkout specific release branches (e.g., `Lakka-v5.x`).

## Building Lakka

### Basic Build Command

The basic syntax for building Lakka is:

```bash
PROJECT=<project> DEVICE=<device> ARCH=<arch> make image
```

Where:
- `PROJECT`: The platform family (e.g., `RPi`, `Generic`, `Rockchip`)
- `DEVICE`: Specific device variant (e.g., `RPi5`, `RPi4`)
- `ARCH`: Architecture (e.g., `aarch64`, `arm`, `x86_64`, `i386`)

### Build Targets

- `make image` - Build the image (most common)
- `make release` - Build and create release files
- `make noobs` - Create NOOBS-compatible image (for Raspberry Pi)
- `make clean` - Clean build artifacts for current project
- `make distclean` - Clean all build artifacts

## Building for Specific Platforms

### Raspberry Pi 5 (Latest Model)

To build Lakka for Raspberry Pi 5:

```bash
PROJECT=RPi DEVICE=RPi5 ARCH=aarch64 make image
```

For Raspberry Pi 5 with composite video output:

```bash
PROJECT=RPi DEVICE=RPi5-Composite ARCH=aarch64 make image
```

### Raspberry Pi 4

```bash
PROJECT=RPi DEVICE=RPi4 ARCH=aarch64 make image
```

### Raspberry Pi 3

```bash
PROJECT=RPi DEVICE=RPi3 ARCH=aarch64 make image
```

### Raspberry Pi 2

```bash
PROJECT=RPi DEVICE=RPi2 ARCH=arm make image
```

### Raspberry Pi 1 / Zero

```bash
PROJECT=RPi DEVICE=RPi ARCH=arm make image
```

### Generic x86_64 PC

```bash
PROJECT=Generic DEVICE=Generic ARCH=x86_64 make image
```

### Generic i386 PC

```bash
PROJECT=Generic DEVICE=Generic ARCH=i386 make image
```

### Rockchip RK3399

```bash
PROJECT=Rockchip DEVICE=RK3399 ARCH=aarch64 make image
```

### Allwinner H6

```bash
PROJECT=Allwinner DEVICE=H6 ARCH=aarch64 make image
```

### Amlogic AMLGX

```bash
PROJECT=Amlogic DEVICE=AMLGX ARCH=aarch64 make image
```

### Nintendo Switch

```bash
PROJECT=L4T DEVICE=Switch ARCH=aarch64 make image
```

### Build with Custom Thread Count

To speed up compilation, specify the number of parallel jobs:

```bash
THREADCOUNT=8 PROJECT=RPi DEVICE=RPi5 ARCH=aarch64 make image
```

By default, the build system uses 2x the number of CPU cores.

## Building All Platforms

To build images for all supported platforms, use the `build_all.sh` script:

### Interactive Dashboard Mode (Default)

```bash
./build_all.sh
```

This mode shows a real-time dashboard of build progress.

### Log Mode

```bash
DASHBOARD_MODE=off ./build_all.sh
```

### Bail Out on First Failure

```bash
BAILOUT_FAILED=yes ./build_all.sh
```

### Custom Refresh Rate

```bash
REFRESH_RATE=1s ./build_all.sh
```

### Combining Options

```bash
DASHBOARD_MODE=yes BAILOUT_FAILED=yes THREADCOUNT=8 ./build_all.sh
```

## Using Docker

Docker provides a consistent build environment across different host systems.

### Build Docker Image

First, create a Docker build environment (using Ubuntu 22.04 as example):

```bash
docker build --pull -t libreelec tools/docker/jammy
```

Available Docker configurations:
- `tools/docker/focal` - Ubuntu 20.04
- `tools/docker/jammy` - Ubuntu 22.04
- `tools/docker/noble` - Ubuntu 24.04
- `tools/docker/bookworm` - Debian 12

### Build Inside Docker Container

#### Build for Default Platform

```bash
docker run --rm --log-driver none -v $(pwd):/build -w /build -it libreelec make image
```

#### Build for Raspberry Pi 5

```bash
docker run --rm --log-driver none \
  -v $(pwd):/build -w /build -it \
  -e PROJECT=RPi -e DEVICE=RPi5 -e ARCH=aarch64 \
  libreelec make image
```

#### Build for Generic x86_64

```bash
docker run --rm --log-driver none \
  -v $(pwd):/build -w /build -it \
  -e PROJECT=Generic -e DEVICE=Generic -e ARCH=x86_64 \
  libreelec make image
```

## Build Output

After a successful build, you'll find the images in the `target/` directory:

```
target/
├── <DEVICE>.<ARCH>/
│   ├── Lakka-<DEVICE>.<ARCH>-<version>.img.gz  (SD card image)
│   ├── Lakka-<DEVICE>.<ARCH>-<version>.tar     (update archive)
│   └── ...
```

For example, Raspberry Pi 5 builds will be in:
```
target/RPi5.aarch64/
```

## Troubleshooting

### Build Fails Due to Missing Dependencies

Run the checkdeps script manually:

```bash
./scripts/checkdeps
```

This will detect and offer to install missing dependencies.

### Out of Disk Space

Building all platforms requires significant disk space (50-100 GB). Clean old builds:

```bash
make distclean
```

Or remove specific build directories:

```bash
rm -rf build.Lakka-*
```

### Build Hangs or Freezes

- Reduce the number of parallel build threads:
  ```bash
  THREADCOUNT=2 PROJECT=RPi DEVICE=RPi5 ARCH=aarch64 make image
  ```

### Package Build Fails

Check the build logs in:
```
build.Lakka-<DEVICE>.<ARCH>*/.threads/logs/
```

### Network Errors During Download

The build system downloads source packages. If downloads fail:
- Check your internet connection
- Retry the build (it will resume from where it failed)
- Check if specific download mirrors are accessible

### Cross-Compilation Issues on ARM64 Hosts

When building Rockchip or Amlogic platforms on native aarch64 systems, ensure you have x86_64 emulation:

```bash
sudo apt install qemu-user-binfmt libc6-amd64-cross
```

### Memory Issues

If you encounter out-of-memory errors:
- Close other applications
- Reduce THREADCOUNT
- Add swap space:
  ```bash
  sudo fallocate -l 8G /swapfile
  sudo chmod 600 /swapfile
  sudo mkswap /swapfile
  sudo swapon /swapfile
  ```

## Getting Help

If you encounter issues not covered in this guide:

- **FAQ**: https://github.com/libretro/Lakka-LibreELEC/wiki/FAQ
- **IRC**: #lakkatv on irc.libera.chat
- **Discord**: https://discord.gg/BNFR4hM
- **Forums**: https://forums.libretro.com/c/libretro/lakka-tv-general

## Contributing

If you've built Lakka successfully and want to contribute:

1. Read [CONTRIBUTING.md](CONTRIBUTING.md)
2. Test your builds on real hardware
3. Submit pull requests to the `devel` branch
4. Join us on IRC or Discord

## License

Lakka is built on LibreELEC and RetroArch, both open-source projects. Please refer to individual component licenses.

---

**Note**: Build times and requirements may vary. The first build will take significantly longer as it downloads and compiles all dependencies. Subsequent builds will be faster due to caching.
