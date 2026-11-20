# ManualJump Add-on: compilación automática

Este staging vive en la rama privada e independiente `manualjump-addon-ci` de `Ys-Andro/JarvisAI-2.0`. La rama `main` no se modifica.

## Ejecución
GitHub Actions está configurado para compilar automáticamente cuando se actualiza esta rama y los cambios afectan al flujo o al archivo de código fuente dividido en partes.

## Artefactos
Si la compilación termina correctamente, GitHub Actions publica:
- `ManualJump.3gx`
- `ManualJump_Azahar_USA_Install.zip`

El artefacto se conserva durante 30 días. Consulta la pestaña **Actions** del repositorio y abre la ejecución correspondiente a la rama `manualjump-addon-ci`.

## Validaciones
- Ejecuta las comprobaciones existentes del proyecto.
- Reconstruye el archivo fuente y valida su SHA-256.
- Compila con el contenedor oficial `devkitpro/devkitarm`.
- Construye `3gxtool` desde su fuente.
- Rechaza el resultado si la cabecera no es `3GX$0002`.

Una compilación correcta no sustituye una prueba dentro de Azahar. Las direcciones de memoria y el hook todavía necesitan validarse en ejecución con la revisión exacta del juego y SingleScreen.
