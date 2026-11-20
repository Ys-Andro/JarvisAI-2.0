# ManualJump Add-on

Complemento independiente en formato 3GX para *The Legend of Zelda: Ocarina of Time 3D* USA Rev 1, diseñado para trabajar junto a **SingleScreen 3.2.2 USA** sin reemplazar ni modificar su parche.

> **Estado:** versión experimental de desarrollo 0.1. La compilación automatizada y la prueba real en Azahar son etapas distintas. No se considera compatible hasta verificar el comportamiento dentro del emulador.

## Objetivo

Añadir salto manual mediante un plugin separado, conservando el parche de SingleScreen:

| Modo | Comportamiento previsto |
|---|---|
| `off` | Controles originales, sin instalar el hook |
| `zr` | ZR ejecuta el salto manual; X conserva su objeto |
| `x` | X ejecuta el salto manual; ZR recibe la función del objeto X |

El complemento está dirigido a:

- Juego: *The Legend of Zelda: Ocarina of Time 3D*
- Región: USA Rev 1
- Title ID: `0004000000033500`
- Emulador objetivo: Azahar en Android

## Compatibilidad y precauciones

- SingleScreen 3.2.2 USA permanece independiente y sin modificaciones.
- No se incluye ni redistribuye el archivo `code.ips` de SingleScreen.
- El plugin debe ser el único 3GX dentro de la carpeta del Title ID.
- La configuración externa `ManualJump.cfg` usa `mode=off` por defecto.
- La coincidencia de la cabecera 3GX y una compilación correcta no demuestran que el hook o las direcciones de memoria sean correctos en ejecución.
- Mantén el modo `off` hasta completar las pruebas en Azahar. Conserva una copia de seguridad de la partida antes de probar los modos activos.

## Configuración

El archivo `ManualJump.cfg` contiene:

```ini
mode=off
```

Los valores permitidos son `off`, `zr` y `x`. Después de editar el archivo, cierra y vuelve a iniciar el juego.

## Instalación prevista

Cuando exista un artefacto de compilación verificado, la estructura será:

```text
sdmc/
└── luma/
    └── plugins/
        └── 0004000000033500/
            ├── ManualJump.3gx
            └── ManualJump.cfg
```

En Azahar activa **Enable 3GX plugin loader** en los ajustes del sistema. No sustituyas, muevas ni combines el `code.ips` de SingleScreen con el plugin.

## Compilación automatizada

La compilación se realiza con GitHub Actions sobre una imagen de devkitARM fijada por digest y con dependencias verificadas por SHA-256.

1. Abre la pestaña **Actions**.
2. Selecciona **ManualJump 3GX Build**.
3. Ejecuta **Run workflow** o deja que se active al cambiar el workflow o algún fragmento de `source-archive/`.
4. Cuando termine correctamente, descarga el artefacto `ManualJump-3GX-Azahar-USA`.

El workflow reconstruye el archivo fuente desde seis fragmentos Base64 y verifica su SHA-256. También descarga versiones fijadas de `3gxtool`, `yaml-cpp` y `dynalo`, comprueba sus hashes, ejecuta las pruebas y valida que el contenedor generado comience con `3GX$0002`, el formato que espera el cargador actual de Azahar.

Antes de compilar aplica tres ajustes de compatibilidad sobre la copia temporal del código fuente: protege la definición de `CUR_PROCESS_HANDLE`, pasa el argumento de hilo como `u32` a `svcCreateThread` y excluye el archivo C vacío `csvc.c` para evitar que colisione con el objeto generado por `csvc.s`. El archivo fuente archivado se conserva sin modificar.

La ejecución aprobada más reciente es [#44698351766](https://github.com/Ys-Andro/JarvisAI-2.0/actions/runs/44698351766). El artefacto se conserva durante 30 días; no es una publicación permanente.

## Estructura del código

El proyecto fuente completo está almacenado en `source-archive/` como seis fragmentos de texto Base64. GitHub Actions los reconstruye, verifica su SHA-256 y extrae el Makefile, linker script, código C/Assembly, pruebas, configuración, documentación y licencia GPL-2.0.

## Estado de validación

- Pruebas de política de botones `off / zr / x`: aprobadas.
- Comprobaciones de sintaxis C: aprobadas.
- Compilación y enlace con devkitARM: aprobados en Actions.
- Cabecera del plugin `3GX$0002`: verificada.
- Paquete de instalación Azahar: generado y comprobado; contiene `ManualJump.3gx` y `ManualJump.cfg` en la ruta del Title ID correcto.
- Verificación adicional de firmas IPS/ExHeader de SingleScreen: omitida en CI porque esos archivos originales no se incluyen en el repositorio.
- Carga y comportamiento del plugin dentro de Azahar: pendientes de prueba real.
- Salto manual, reasignación X/ZR y conservación de la lógica SingleScreen durante el juego: pendientes de validación.

**No consideres validado el salto por el hecho de que el plugin compile.** Mantén `mode=off` hasta completar las pruebas dentro de Azahar.

## Atribución y licencia

El código del complemento se distribuye bajo GPL-2.0-or-later. Consulta el archivo `LICENSE-GPL-2.0.txt` incluido en el archivo fuente.

Referencias técnicas consultadas:
- [Azahar](https://github.com/azahar-emu/azahar)
- [Ocarina ReTuned / OOT3DHud](https://github.com/OTPR26/OOT3DHud)
- [CTRComposer](https://github.com/samaBR85/CTRComposer)
- [devkitPro](https://devkitpro.org/)
