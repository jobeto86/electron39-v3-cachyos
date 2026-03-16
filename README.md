# Electron 41 (x86-64-v3) for CachyOS

Optimized build of **Electron 41** for **CachyOS** using the **x86-64-v3** microarchitecture level.

## Quick Installation (Binary)

Install the pre-compiled binary from Releases:

```bash
sudo pacman -U https://github.com/jobeto86/electron39-v3-cachyos/releases/download/v41.0.2-v3/electron41-41.0.2-1-x86_64.pkg.tar.zst
```

Or from AUR:

```bash
yay -S electron41-v3-bin
```

## Building from source

### Prerequisites

- `base-devel`, `git`
- CachyOS or Arch Linux with up-to-date toolchain (clang, llvm, rust, etc.)
- ~50 GB disk space, ~16 GB RAM recommended

### First-time setup

After cloning, you must generate the managed source list (150+ Chromium git deps):

```bash
# Install python-requests if not present
sudo pacman -S python-requests

# Generate managed sources and checksums in PKGBUILD
bash -c 'source PKGBUILD; _update_sources'
```

### Build

```bash
makepkg -si
```

## What's different from vanilla Arch electron?

- **`-march=x86-64-v3`** compiler flags (AVX2, BMI2, FMA, etc.)
- **`thin_lto_enable_optimizations=true`** for better ThinLTO
- **`increase-fortify-level.patch`** for hardened builds
- **`fix-sys-seccomp-glibc243.patch`** for glibc 2.43 compatibility
- `fatal_linker_warnings=false` to avoid build failures on warnings

## Updating to a new Electron version

1. Update `pkgver` in PKGBUILD
2. Update `_gcc_patches` to match the Chromium major version
3. Run `bash -c 'source PKGBUILD; _update_sources'`
4. Test build with `makepkg -s`
5. Verify patches still apply cleanly; update as needed

## Technical Details

| | |
|---|---|
| **Electron** | 41.0.2 |
| **Chromium** | ~146.x |
| **Node.js** | 24.14.0 |
| **Arch level** | x86-64-v3 |
| **Target OS** | CachyOS / Arch Linux |
| **Compiler** | Clang/LLVM |

---
*Maintained by [jobeto86](https://github.com/jobeto86)*
