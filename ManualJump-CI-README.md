# ManualJump Add-on: compilación automática

Este staging está en una rama privada e independiente: `manualjump-addon-ci`. No modifica la rama principal.

## Ejecutar
El flujo `.github/workflows/manualjump-build.yml` se ejecuta al actualizar esta rama y admite ejecución manual desde **Actions → Build ManualJump 3GX → Run workflow**.

## Artefactos
Si la compilación termina correctamente, GitHub Actions publica `ManualJump.3gx` y `ManualJump_Azahar_USA_Install.zip` en un artefacto con retención de 30 días.

## Validaciones
- Ejecuta las comprobaciones existentes del proyecto.
- Compila con el contenedor oficial `devkitpro/devkitarm`.
- Construye `3gxtool` desde su fuente.
- Rechaza el resultado si la cabecera no es `3GX$0002`.

Una compilación correcta no sustituye una prueba dentro de Azahar. Las direcciones de memoria y el hook todavía necesitan validarse en ejecución con la revisión exacta del juego y SingleScreen.
