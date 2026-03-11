# Electron 39 (x86-64-v3) for CachyOS

This repository provides an optimized build of **Electron 39** specifically compiled for **CachyOS** using the **x86-64-v3** architecture.

## Why this exists?
Standard Electron builds sometimes face compatibility or performance issues on rolling-release distributions with advanced instruction sets like CachyOS. This build is verified to work with tools like `antigravity`.

## 🚀 Quick Installation (Binary)

You don't need to compile it! Install the latest pre-compiled binary directly from the Releases page:

```bash
sudo pacman -U https://github.com/jobeto86/electron39-v3-cachyos/releases/download/v39.5.2-v3/electron39-39.5.2-1-x86_64.pkg.tar.zst
```

## 🛠 Manual Build
If you prefer to build it yourself:

1. Clone this repo.
2. Ensure you have `base-devel` and `cachyos-settings` (for v3 optimizations).
3. Run:
   ```bash
   makepkg -si
   ```

## Technical Details
- **Version:** 39.5.2
- **Architecture:** x86-64-v3 (Optimized for modern CPUs)
- **OS Target:** CachyOS / Arch Linux
- **Compiler:** Clang/LLVM with CachyOS flags.

---
*Maintained by [jobeto86](https://github.com/jobeto86)*
