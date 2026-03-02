# Electron 39 Optimized for CachyOS (x86-64-v3)

[English](#english) | [Español](#español)

---

## English

This project provides a specialized build of **Electron 39** optimized for **CachyOS** and modern CPUs supporting the `x86-64-v3` instruction set (AVX, AVX2, FMA, BMI2). 

### 🚀 Features
- **CPU Optimization:** Compiled with `-march=x86-64-v3` for improved performance in V8 and rendering tasks.
- **Glibc 2.43+ Compatibility:** Includes a critical patch for `SYS_SECCOMP` macro collision, fixing compilation failures on the latest Arch/CachyOS systems.
- **Ninja Workflow:** Documentation on how to use `ninja` directly to avoid losing progress on failed builds.
- **System Libraries:** Configured to use system libraries where possible for better integration.

### 🛠️ How to Build
1. Clone this repository.
2. Run `makepkg -s` to prepare the source and dependencies.
3. If the build fails during the long compilation phase, use the `GUIA_NINJA.md` instructions to resume using `ninja` without restarting from scratch.
4. Install with `sudo pacman -U electron39-*.pkg.tar.zst`.

---

## Español

Este proyecto ofrece una compilación especializada de **Electron 39** optimizada para **CachyOS** y CPUs modernas que soportan el conjunto de instrucciones `x86-64-v3` (AVX, AVX2, FMA, BMI2).

### 🚀 Características
- **Optimización de CPU:** Compilado con `-march=x86-64-v3` para un mejor rendimiento en el motor V8 y tareas de renderizado.
- **Compatibilidad con Glibc 2.43+:** Incluye un parche crítico para la colisión de la macro `SYS_SECCOMP`, solucionando fallos de compilación en los sistemas Arch/CachyOS más recientes.
- **Flujo de Trabajo Ninja:** Documentación sobre cómo usar `ninja` directamente para evitar perder el progreso en compilaciones fallidas.
- **Librerías del Sistema:** Configurado para usar librerías del sistema donde sea posible para una mejor integración.

### 🛠️ Cómo Compilar
1. Clona este repositorio.
2. Ejecuta `makepkg -s` para preparar las fuentes y dependencias.
3. Si la compilación falla durante la fase larga, sigue las instrucciones en `GUIA_NINJA.md` para retomar usando `ninja` sin empezar desde cero.
4. Instala con `sudo pacman -U electron39-*.pkg.tar.zst`.

---

## Acknowledgments / Agradecimientos
- **Community:** Developed for the CachyOS community.
- **Fix:** Special thanks for the `glibc 2.43` SYS_SECCOMP fix.
