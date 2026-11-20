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

La compilación se realiza con GitHub Actions usando el contenedor devkitARM, libctru y `3gxtool`.

1. Abre la pestaña **Actions**.
2. Selecciona **ManualJump 3GX Build**.
3. Ejecuta **Run workflow** o consulta la ejecución automática tras un cambio en el workflow o el archivo fuente.
4. Cuando la ejecución termine correctamente, descarga el artefacto `ManualJump-3GX-Azahar-USA`.

El flujo verifica el SHA-256 del archivo fuente, ejecuta las pruebas del proyecto, compila el plugin y comprueba que el contenedor comience con `3GX$0002`. El paquete instalable se publica solo si todas esas etapas terminan correctamente. Los artefactos se conservan durante 30 días.

## Estructura del código

El proyecto fuente completo está almacenado en `source-archive/` como seis fragmentos de texto Base64. GitHub Actions los reconstruye, verifica su SHA-256 y extrae el Makefile, linker script, código C/Assembly, pruebas, configuración, documentación y licencia GPL-2.0.

## Estado de validación

- Pruebas de política de botones `off / zr / x`: aprobadas.
- Comprobaciones de sintaxis C: aprobadas.
- Verificación de firma binaria con los archivos originales IPS y ExHeader: pendiente de repetir con esos archivos presentes en el entorno de validación.
- Compilación 3GX con devkitARM: en proceso de validación mediante Actions.
- Carga y comportamiento del plugin en Azahar: pendiente.
- Validación práctica del salto, reasignación de X/ZR y conservación de la lógica SingleScreen: pendiente.

## Atribución y licencia

El código del complemento se distribuye bajo GPL-2.0-or-later. Consulta el archivo `LICENSE-GPL-2.0.txt` incluido en el archivo fuente.

Referencias técnicas consultadas:
- [Azahar](https://github.com/azahar-emu/azahar)
- [Ocarina ReTuned / OOT3DHud](https://github.com/OTPR26/OOT3DHud)
- [CTRComposer](https://github.com/samaBR85/CTRComposer)
- [devkitPro](https://devkitpro.org/)
