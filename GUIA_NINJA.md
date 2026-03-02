# electron39 — Guía de compilación directa con ninja

## IMPORTANTE: NO usar `makepkg` para recompilar
`makepkg` re-ejecuta `prepare()` cada vez, lo que regenera todo y pierde los `.o` compilados.
En su lugar, usar ninja directamente.

## Paso 1: Matar makepkg si está corriendo
```bash
pkill -f makepkg
# Esperar unos segundos
sleep 3
# Verificar que murió
pgrep -f "makepkg|ninja" && echo "AÚN CORRIENDO" || echo "DETENIDO"
```

## Paso 2: Aplicar parche SYS_SECCOMP (glibc 2.43)
El archivo `sandbox/linux/system_headers/linux_seccomp.h` define `SYS_SECCOMP` como macro,
lo que colisiona con el enum de glibc 2.43+. Este sed lo arregla:

```bash
cd ./src/src

sed -i '/^#ifndef SYS_SECCOMP$/,/^#endif$/c\
#if !defined(SYS_SECCOMP) \&\& !defined(__GLIBC_PREREQ)\
#define SYS_SECCOMP 1\
#elif !defined(SYS_SECCOMP) \&\& defined(__GLIBC_PREREQ)\
#if !__GLIBC_PREREQ(2, 43)\
#define SYS_SECCOMP 1\
#endif\
#endif' sandbox/linux/system_headers/linux_seccomp.h
```

### Verificar que el parche se aplicó:
```bash
grep -A8 "SYS_SECCOMP" sandbox/linux/system_headers/linux_seccomp.h
```
Debe mostrar las líneas con `__GLIBC_PREREQ`, NO el viejo `#ifndef SYS_SECCOMP`.

## Paso 3: Lanzar ninja directamente
```bash
cd ./src/src

# Con prioridad baja (si estás usando la máquina):
nice -n 19 ninja -C out/Release electron 2>&1 | tee -a ../../build-ninja.log

# Con prioridad normal (si NO estás usando la máquina):
ninja -C out/Release electron 2>&1 | tee -a ../../build-ninja.log
```

Ninja detecta los `.o` ya compilados y retoma desde donde quedó.
Si falla por cualquier otro error, corrige y vuelve a ejecutar el mismo comando ninja.
**NO relances makepkg.**

## Paso 4: Monitorear progreso
```bash
# Ver último target
grep -oE '\[[0-9]+/[0-9]+\]' ./build-ninja.log | tail -1

# Ver si sigue corriendo
pgrep -f ninja && echo "CORRIENDO" || echo "TERMINADO"

# Ver errores
tail -20 ./build-ninja.log | grep -i "FAILED\|error"

# Pausar/reanudar
pkill -STOP -f ninja    # Pausar
pkill -CONT -f ninja    # Reanudar

# Cambiar prioridad
sudo renice -n 0 -p $(pgrep -f "ninja" | head -1)    # Normal/alta
sudo renice -n 19 -p $(pgrep -f "ninja" | head -1)   # Baja
```

## Paso 5: Cuando ninja termine, empaquetar
Una vez que ninja termine sin errores, empaquetar con makepkg SIN recompilar:

```bash
cd .
makepkg --noconfirm -e -R
```

La flag `-e` salta prepare/build y usa lo que ya compiló ninja.
La flag `-R` re-empaqueta.

## Paso 6: Instalar el paquete
```bash
sudo pacman -U electron39-*.pkg.tar.zst
```

## Resumen de estado
- **Progreso anterior**: llegó a 29,200/43,813 antes de fallar por SYS_SECCOMP
- **Error**: conflicto de macro SYS_SECCOMP entre chromium y glibc 2.43 (CachyOS)
- **PKGBUILD modificado**: `nodejs-lts-iron` → `nodejs` en makedepends
- **Disco usado**: ~60GB (chromium + sub-repos)
- **Target de ninja**: `electron` (alias `electron_app`)
- **Directorio de build**: `src/src/out/Release/`

## Si algo sale mal
- **Otro error de compilación**: corregir el archivo fuente directamente y relanzar `ninja -C out/Release electron`. Ninja retoma donde quedó.
- **Necesitas regenerar el build**: `cd src/src && gn gen out/Release` y luego `ninja -C out/Release electron`
- **Quieres empezar desde cero**: `makepkg --noconfirm -C` (limpia todo y recompila)
