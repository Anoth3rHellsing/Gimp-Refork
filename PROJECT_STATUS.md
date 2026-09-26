# GIMP Refork - Estado del Proyecto

**Fecha:** 2026-09-09
**Objetivo:** Crear un refork de GIMP con interfaz moderna estilo Adobe para facilitar la transición desde Photoshop.

## Ubicación Actual

Todos los archivos del proyecto están en: `D:\ClaudeCodeD\Gimp Refork\`

### Contenido
- `msys64/` — Instalación completa de MSYS2 (actualizada, sistema base listo)
- `msys2-installer.exe` — Instalador original (backup, ~83 MB)
- `PROJECT_STATUS.md` — Este archivo

## Lo que se completó

1. ✅ Descarga e instalación de MSYS2 en entorno MINGW64
2. ✅ Actualización completa del sistema base MSYS2 (pacman -Syu x2)
3. ✅ Verificación de espacio en disco y reubicación a D: por falta de espacio en C:
4. ✅ Limpieza de carpeta original en `C:\Users\nicol\Documents\ClaudeCode\Gimp Refork`

## Bloqueo Actual: Dependencias de GIMP no disponibles

**Problema:** MSYS2 ya no distribuye paquetes binarios precompilados de las dependencias centrales de GIMP:
- `mingw-w64-x86_64-gegl` — **NO EXISTE** en repos actuales
- `mingw-w64-x86_64-babl` — **NO EXISTE** en repos actuales
- Otros paquetes con nombres incorrectos: libwebp, libtiff, libpng, libjpeg-turbo (los nombres reales difieren)

**Impacto:** No es posible compilar GIMP directamente con `pacman -S`. Se requiere compilar BABL y GEGL desde el fuente primero.

## Pasos para Retomar

### Opción A: Compilar desde el fuente (recomendado pero largo)
1. Clonar repositorios de BABL y GEGL desde GitLab GNOME
2. Compilar BABL → instalar en prefix local
3. Compilar GEGL (depende de BABL) → instalar en prefix local
4. Configurar variables de entorno PKG_CONFIG_PATH para apuntar al prefix local
5. Instalar dependencias restantes con nombres correctos (buscar con `pacman -Ss`)
6. Clonar GIMP desde `https://gitlab.gnome.org/GNOME/gimp`
7. Compilar GIMP con meson/ninja apuntando al prefix local
8. Empaquetar ejecutable y DLLs en subcarpeta `build/`

### Opción B: Usar script de compilación oficial de GIMP
- El proyecto GIMP mantiene scripts de cross-compilation en `build/windows/` dentro del repo
- Estos scripts automatizan la compilación de BABL, GEGL y GIMP
- Requieren MSYS2 MINGW64 funcional (ya instalado)
- Referencia: https://gitlab.gnome.org/GNOME/gimp/-/tree/master/build/windows

### Opción C: Prototipo UI web como alternativa inmediata
- Crear interfaz moderna estilo Adobe en HTML/CSS/JS
- Sirve como diseño interactivo y especificación visual
- Puede convertirse en app Electron posteriormente
- No requiere compilación de GIMP

## Comandos Útiles para Retomar

```bash
# Abrir shell MINGW64
D:\ClaudeCodeD\Gimp Refork\msys64\mingw64.exe

# Buscar paquete correcto
pacman -Ss <nombre>

# Verificar gcc
gcc --version

# Clonar GIMP
git clone https://gitlab.gnome.org/GNOME/gimp.git
```

## Notas Técnicas
- MSYS2 fue instalado el 2026-09-09 con el instalador `msys2-x86_64-20240727.exe`
- El sistema base está actualizado pero las dependencias de GIMP NO están instaladas
- La carpeta original en C: fue limpiada tras mover todo a D:
- Espacio liberado en C: ~3 GB (MSYS2 + instalador)