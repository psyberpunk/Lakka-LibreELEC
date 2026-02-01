# Lakka Build Quick Reference

Quick reference for building Lakka. For detailed instructions, see [BUILD.md](BUILD.md) or [BUILD.es.md](BUILD.es.md) (Spanish).

## Prerequisites

```bash
# Ubuntu/Debian
sudo apt install -y bash bc bzip2 curl diffutils gawk gcc gperf gzip file \
  lsdiff lzop make patch perl rdfind rsync sed tar unzip wget xz-utils zip zstd \
  g++ xsltproc default-jre python3 libncurses5-dev libc6-dev \
  libjson-perl libparse-yapp-perl libxml-parser-perl git
```

## Quick Start

```bash
# Clone repository
git clone https://github.com/libretro/Lakka-LibreELEC.git
cd Lakka-LibreELEC

# Checkout development branch
git checkout devel

# Build for Raspberry Pi 5
PROJECT=RPi DEVICE=RPi5 ARCH=aarch64 make image

# Build for Generic x86_64 PC
PROJECT=Generic DEVICE=Generic ARCH=x86_64 make image
```

## All Supported Builds

### Raspberry Pi Models
```bash
# Raspberry Pi 5 (Latest)
PROJECT=RPi DEVICE=RPi5 ARCH=aarch64 make image

# Raspberry Pi 4
PROJECT=RPi DEVICE=RPi4 ARCH=aarch64 make image

# Raspberry Pi 3
PROJECT=RPi DEVICE=RPi3 ARCH=aarch64 make image

# Raspberry Pi 2
PROJECT=RPi DEVICE=RPi2 ARCH=arm make image

# Raspberry Pi 1 / Zero
PROJECT=RPi DEVICE=RPi ARCH=arm make image
```

### Generic PC
```bash
# x86_64
PROJECT=Generic DEVICE=Generic ARCH=x86_64 make image

# i386
PROJECT=Generic DEVICE=Generic ARCH=i386 make image
```

### Rockchip
```bash
PROJECT=Rockchip DEVICE=RK3399 ARCH=aarch64 make image
PROJECT=Rockchip DEVICE=RK3328 ARCH=aarch64 make image
PROJECT=Rockchip DEVICE=RK3288 ARCH=arm make image
```

### Allwinner
```bash
PROJECT=Allwinner DEVICE=H6 ARCH=aarch64 make image
PROJECT=Allwinner DEVICE=H616 ARCH=aarch64 make image
PROJECT=Allwinner DEVICE=H5 ARCH=aarch64 make image
PROJECT=Allwinner DEVICE=H3 ARCH=arm make image
PROJECT=Allwinner DEVICE=A64 ARCH=aarch64 make image
```

### Amlogic
```bash
PROJECT=Amlogic DEVICE=AMLGX ARCH=aarch64 make image
```

### NXP
```bash
PROJECT=NXP DEVICE=iMX8 ARCH=aarch64 make image
PROJECT=NXP DEVICE=iMX6 ARCH=arm make image
```

### Other
```bash
# Nintendo Switch
PROJECT=L4T DEVICE=Switch ARCH=aarch64 make image

# Samsung Exynos
PROJECT=Samsung DEVICE=Exynos ARCH=arm make image

# Ayn Odin
PROJECT=Ayn DEVICE=Odin ARCH=aarch64 make image
```

## Build All Platforms

```bash
./build_all.sh
```

## Docker Build

```bash
# Build Docker image (Ubuntu 22.04)
docker build --pull -t libreelec tools/docker/jammy

# Build Lakka for RPi5 in Docker
docker run --rm --log-driver none \
  -v $(pwd):/build -w /build -it \
  -e PROJECT=RPi -e DEVICE=RPi5 -e ARCH=aarch64 \
  libreelec make image
```

## Output Location

Images are in: `target/<DEVICE>.<ARCH>/`

Example: `target/RPi5.aarch64/Lakka-RPi5.aarch64-*.img.gz`

## Useful Commands

```bash
# Clean current build
make clean

# Clean all builds
make distclean

# Build with custom thread count
THREADCOUNT=8 PROJECT=RPi DEVICE=RPi5 ARCH=aarch64 make image

# Check dependencies
./scripts/checkdeps
```

## Getting Help

- Documentation: [BUILD.md](BUILD.md)
- FAQ: https://github.com/libretro/Lakka-LibreELEC/wiki/FAQ
- IRC: #lakkatv on irc.libera.chat
- Discord: https://discord.gg/BNFR4hM
