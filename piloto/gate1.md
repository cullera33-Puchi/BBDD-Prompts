# Versión 1. Piloto de exportación

Objetivo: validar la plantilla y el prompt de exportación con tres proyectos reales antes de escribir código.

## Ficheros

| Fichero | Uso |
|---|---|
| `prompt-exportacion.txt` | Texto completo para pegar en un chat nuevo del proyecto de Claude. Ya incluye la plantilla |
| `plantilla-proyecto.md` | Plantilla sola, para revisar o corregir a mano |
| `gate1.md` | Este fichero: proyectos del piloto, lista de control y registro de ajustes |

Los MD exportados no se suben a este repositorio, que solo contiene código. Se guardan en local o en la carpeta de Drive y, para revisarlos, se pegan en el chat de Claude Code.

## Proyectos del piloto

| Tipo | Proyecto de Claude | id propuesto | Exportado | Revisado | Validado |
|---|---|---|---|---|---|
| Cliente | | | [ ] | [ ] | [ ] |
| Interno | | | [ ] | [ ] | [ ] |
| Personal | | | [ ] | [ ] | [ ] |

## Pasos por proyecto

1. Abrir un chat nuevo dentro del proyecto de Claude.
2. Pegar el contenido de `prompt-exportacion.txt`.
3. Revisar el MD con la lista de control de abajo y pedir las correcciones en el mismo chat.
4. Guardar el MD final como `<id>.md`.
5. Anotar en el registro de ajustes cualquier cambio que convenga llevar a la plantilla o al prompt.

## Lista de control

### Cabecera

- [ ] Empieza por `---` en la primera línea y cierra con `---`.
- [ ] Están los campos obligatorios: `id`, `proyecto`, `cliente`, `tipo`, `estado`, `actualizado`.
- [ ] `id` en mayúsculas, sin tildes ni espacios, con el patrón `PRJ-<CLIENTE>-<TEMA>`.
- [ ] `tipo` es uno de: implantación, AMS, propuesta, upgrade, interno o personal.
- [ ] `estado` es uno de: activo, cerrado o en propuesta.
- [ ] `actualizado` con formato `AAAA-MM-DD`.
- [ ] `modulos`, `normativa` y `etiquetas` son listas entre corchetes, aunque estén vacías.
- [ ] Ningún valor contiene dos puntos seguidos de espacio sin comillas (rompe el YAML).

### Contenido

- [ ] No se han cambiado ni reordenado los encabezados de la plantilla.
- [ ] Las decisiones son solo las que tomó Enric. Ninguna propuesta de Claude no aceptada figura como decisión.
- [ ] Cada decisión tiene fecha y motivo.
- [ ] Los conceptos sirven en otro proyecto y usan los prefijos NORM, TEC, PLA, MET, DEC o GLO.
- [ ] Los prompts están en versión reutilizable: sin nombres de cliente, importes ni datos personales.
- [ ] Las personas aparecen por su rol, nunca por nombre.
- [ ] No hay datos inventados: los campos sin información están vacíos.
- [ ] El bloque de código llega entero, sin cortes. Si el proyecto es grande, comprobar que no faltan secciones al final.

## Registro de ajustes

| N.º | Proyecto | Problema detectado | Ajuste propuesto | Aplicado en |
|---|---|---|---|---|
| 1 | Todos | Un bloque de código de tres acentos graves se rompe si el MD contiene otro bloque de código | Pedir delimitador de cuatro acentos graves (regla 7 del prompt) | CLAUDE.md y `prompt-exportacion.txt` |

## Gate 1

Se cierra cuando los tres proyectos están validados y los ajustes del registro están aplicados en `CLAUDE.md` y en los ficheros de esta carpeta. Al cerrarlo, actualizar el apartado 13 de `CLAUDE.md` y empezar la versión 2.
