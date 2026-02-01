# Compilar Lakka desde el Código Fuente

Esta guía proporciona instrucciones completas para compilar Lakka para todos los chipsets soportados, incluyendo Raspberry Pi 5.

## Tabla de Contenidos

- [Requisitos del Sistema](#requisitos-del-sistema)
- [Plataformas Soportadas](#plataformas-soportadas)
- [Instalación de Dependencias](#instalación-de-dependencias)
- [Obtener el Código Fuente](#obtener-el-código-fuente)
- [Compilar Lakka](#compilar-lakka)
- [Compilar para Plataformas Específicas](#compilar-para-plataformas-específicas)
- [Compilar Todas las Plataformas](#compilar-todas-las-plataformas)
- [Usar Docker](#usar-docker)
- [Solución de Problemas](#solución-de-problemas)

## Requisitos del Sistema

### Especificaciones del Sistema Recomendadas

- **CPU**: Procesador multi-núcleo (se recomiendan 4+ núcleos)
- **RAM**: 8 GB mínimo, se recomiendan 16 GB o más
- **Espacio en Disco**: 50-100 GB de espacio libre (varía según la plataforma)
- **SO**: Ubuntu LTS (20.04, 22.04, o 24.04), Debian 12, o distribución Linux similar

### Estimaciones de Tiempo de Compilación

Los tiempos de compilación varían significativamente según el hardware y la plataforma objetivo:
- **Una sola plataforma**: 1-4 horas en hardware moderno
- **Todas las plataformas**: 24-48 horas o más

## Plataformas Soportadas

Lakka soporta la compilación para los siguientes chipsets y dispositivos:

### Plataformas basadas en ARM

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

### Plataformas basadas en x86

#### Generic (PC)
- **Generic** (i386)
- **Generic** (x86_64)
- **Wayland** (x86_64)
- **X11** (x86_64)

### Otras Plataformas

#### L4T (NVIDIA Tegra)
- **Switch** (Nintendo Switch) (aarch64)

## Instalación de Dependencias

### Ubuntu / Debian

En Ubuntu LTS (20.04, 22.04, 24.04) o Debian 12, el sistema de compilación detectará automáticamente y te pedirá instalar las dependencias faltantes. Sin embargo, puedes instalar manualmente las dependencias principales:

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

## Obtener el Código Fuente

### Clonar el Repositorio

Para desarrollo y contribuciones, clona el repositorio oficial de Lakka:

```bash
git clone https://github.com/libretro/Lakka-LibreELEC.git
cd Lakka-LibreELEC
```

### Cambiar a la Rama de Desarrollo

La rama de desarrollo es `devel`:

```bash
git fetch origin devel:devel
git checkout devel
```

Para versiones estables, cambia a ramas de versiones específicas (ej., `Lakka-v5.x`).

## Compilar Lakka

### Comando de Compilación Básico

La sintaxis básica para compilar Lakka es:

```bash
PROJECT=<proyecto> DEVICE=<dispositivo> ARCH=<arquitectura> make image
```

Donde:
- `PROJECT`: La familia de plataforma (ej., `RPi`, `Generic`, `Rockchip`)
- `DEVICE`: Variante de dispositivo específico (ej., `RPi5`, `RPi4`)
- `ARCH`: Arquitectura (ej., `aarch64`, `arm`, `x86_64`, `i386`)

### Objetivos de Compilación

- `make image` - Compilar la imagen (más común)
- `make release` - Compilar y crear archivos de versión
- `make noobs` - Crear imagen compatible con NOOBS (para Raspberry Pi)
- `make clean` - Limpiar artefactos de compilación para el proyecto actual
- `make distclean` - Limpiar todos los artefactos de compilación

## Compilar para Plataformas Específicas

### Raspberry Pi 5 (Modelo Más Reciente)

Para compilar Lakka para Raspberry Pi 5:

```bash
PROJECT=RPi DEVICE=RPi5 ARCH=aarch64 make image
```

Para Raspberry Pi 5 con salida de video compuesto:

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

### PC Genérico x86_64

```bash
PROJECT=Generic DEVICE=Generic ARCH=x86_64 make image
```

### PC Genérico i386

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

### Compilar con Número Personalizado de Hilos

Para acelerar la compilación, especifica el número de trabajos paralelos:

```bash
THREADCOUNT=8 PROJECT=RPi DEVICE=RPi5 ARCH=aarch64 make image
```

Por defecto, el sistema de compilación usa 2x el número de núcleos de CPU.

## Compilar Todas las Plataformas

Para compilar imágenes para todas las plataformas soportadas, usa el script `build_all.sh`:

### Modo Panel Interactivo (Por Defecto)

```bash
./build_all.sh
```

Este modo muestra un panel en tiempo real del progreso de compilación.

### Modo de Registro

```bash
DASHBOARD_MODE=off ./build_all.sh
```

### Salir al Primer Fallo

```bash
BAILOUT_FAILED=yes ./build_all.sh
```

### Tasa de Actualización Personalizada

```bash
REFRESH_RATE=1s ./build_all.sh
```

### Combinando Opciones

```bash
DASHBOARD_MODE=yes BAILOUT_FAILED=yes THREADCOUNT=8 ./build_all.sh
```

### Compilar Todos los Paquetes Incluyendo Addons

Por defecto, Lakka compila aproximadamente **443 paquetes base** que forman el sistema central. Sin embargo, el repositorio completo de paquetes contiene aproximadamente **1163 paquetes** en total, lo que incluye paquetes addon opcionales.

Para compilar **todos los paquetes** (base + addons):

```bash
MTADDONBUILD=yes ./build_all.sh
```

Para una sola plataforma con todos los paquetes:

```bash
MTADDONBUILD=yes PROJECT=RPi DEVICE=RPi5 ARCH=aarch64 make image
```

**Tipos de Paquetes:**
- **Paquetes base** (~443): Componentes del sistema central requeridos para que Lakka funcione
- **Paquetes addon** (~720): Paquetes opcionales incluyendo núcleos de emuladores, juegos, herramientas y utilidades

**Nota:** Compilar todos los paquetes aumenta significativamente el tiempo de compilación y los requisitos de espacio en disco (se recomiendan más de 100 GB).

## Usar Docker

Docker proporciona un entorno de compilación consistente en diferentes sistemas host.

### Compilar Imagen de Docker

Primero, crea un entorno de compilación Docker (usando Ubuntu 22.04 como ejemplo):

```bash
docker build --pull -t libreelec tools/docker/jammy
```

Configuraciones de Docker disponibles:
- `tools/docker/focal` - Ubuntu 20.04
- `tools/docker/jammy` - Ubuntu 22.04
- `tools/docker/noble` - Ubuntu 24.04
- `tools/docker/bookworm` - Debian 12

### Compilar Dentro del Contenedor Docker

#### Compilar para Plataforma por Defecto

```bash
docker run --rm --log-driver none -v $(pwd):/build -w /build -it libreelec make image
```

#### Compilar para Raspberry Pi 5

```bash
docker run --rm --log-driver none \
  -v $(pwd):/build -w /build -it \
  -e PROJECT=RPi -e DEVICE=RPi5 -e ARCH=aarch64 \
  libreelec make image
```

#### Compilar para Generic x86_64

```bash
docker run --rm --log-driver none \
  -v $(pwd):/build -w /build -it \
  -e PROJECT=Generic -e DEVICE=Generic -e ARCH=x86_64 \
  libreelec make image
```

## Salida de Compilación

Después de una compilación exitosa, encontrarás las imágenes en el directorio `target/`:

```
target/
├── <DEVICE>.<ARCH>/
│   ├── Lakka-<DEVICE>.<ARCH>-<version>.img.gz  (imagen de tarjeta SD)
│   ├── Lakka-<DEVICE>.<ARCH>-<version>.tar     (archivo de actualización)
│   └── ...
```

Por ejemplo, las compilaciones de Raspberry Pi 5 estarán en:
```
target/RPi5.aarch64/
```

## Solución de Problemas

### La Compilación Falla por Dependencias Faltantes

Ejecuta el script checkdeps manualmente:

```bash
./scripts/checkdeps
```

Esto detectará y ofrecerá instalar las dependencias faltantes.

### Sin Espacio en Disco

Compilar todas las plataformas requiere espacio significativo en disco (50-100 GB). Limpia compilaciones antiguas:

```bash
make distclean
```

O elimina directorios de compilación específicos:

```bash
rm -rf build.Lakka-*
```

### La Compilación se Cuelga o se Congela

- Reduce el número de hilos de compilación paralelos:
  ```bash
  THREADCOUNT=2 PROJECT=RPi DEVICE=RPi5 ARCH=aarch64 make image
  ```

### Falla la Compilación de un Paquete

Revisa los registros de compilación en:
```
build.Lakka-<DEVICE>.<ARCH>*/.threads/logs/
```

### Errores de Red Durante la Descarga

El sistema de compilación descarga paquetes fuente. Si las descargas fallan:
- Verifica tu conexión a internet
- Reintenta la compilación (se reanudará desde donde falló)
- Verifica si los espejos de descarga específicos son accesibles

### Problemas de Compilación Cruzada en Hosts ARM64

Al compilar plataformas Rockchip o Amlogic en sistemas nativos aarch64, asegúrate de tener emulación x86_64:

```bash
sudo apt install qemu-user-binfmt libc6-amd64-cross
```

### Problemas de Memoria

Si encuentras errores de falta de memoria:
- Cierra otras aplicaciones
- Reduce THREADCOUNT
- Agrega espacio de intercambio:
  ```bash
  sudo fallocate -l 8G /swapfile
  sudo chmod 600 /swapfile
  sudo mkswap /swapfile
  sudo swapon /swapfile
  ```

## Obtener Ayuda

Si encuentras problemas no cubiertos en esta guía:

- **FAQ**: https://github.com/libretro/Lakka-LibreELEC/wiki/FAQ
- **IRC**: #lakkatv en irc.libera.chat
- **Discord**: https://discord.gg/BNFR4hM
- **Foros**: https://forums.libretro.com/c/libretro/lakka-tv-general

## Contribuir

Si has compilado Lakka exitosamente y quieres contribuir:

1. Lee [CONTRIBUTING.md](CONTRIBUTING.md)
2. Prueba tus compilaciones en hardware real
3. Envía pull requests a la rama `devel`
4. Únete a nosotros en IRC o Discord

## Licencia

Lakka está construido sobre LibreELEC y RetroArch, ambos proyectos de código abierto. Por favor, consulta las licencias de los componentes individuales.

---

**Nota**: Los tiempos y requisitos de compilación pueden variar. La primera compilación tomará significativamente más tiempo ya que descarga y compila todas las dependencias. Las compilaciones subsecuentes serán más rápidas debido al almacenamiento en caché.
