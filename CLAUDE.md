# Base de conocimiento de proyectos

Web personal para consultar y alimentar una base de conocimiento con la información y los prompts de todos los proyectos de Claude de Enric. Escritorio como vista principal y vista móvil para consultar, lanzar prompts y añadir contenido.

Este fichero es la especificación de trabajo para Claude Code. Léelo entero antes de empezar cada versión.

## 1. Objetivo

1. Consolidar en ficheros Markdown, uno por proyecto, toda la información útil de cada proyecto de Claude: contexto, decisiones, conceptos reutilizables, prompts, entregables, lecciones y pendientes.
2. Guardar esos MD en una carpeta de Google Drive, que es la única fuente de verdad.
3. Ofrecer una web para buscar, filtrar, leer, añadir y editar ese contenido, y para lanzar los prompts en Claude con su contexto.
4. Servir de base a Claude: el conector de Google Drive le permite leer los MD, y los prompts más usados acabarán convertidos en skills.

La base tiene dos usuarios: Enric, desde la web, y Claude, que la consulta cuando se le pide cuadrar un JOB, revisar un plan de proyecto o calcular el coste de implantación por perfiles.

## 2. Reglas para Claude Code

1. Trabaja por versiones, en el orden del roadmap (apartado 12). No empieces una versión sin cerrar los criterios de aceptación de la anterior.
2. Sin servidor propio ni base de datos. La web es estática y habla directamente con la API de Google Drive desde el navegador.
3. Sin paso de compilación: HTML, CSS y JavaScript con módulos ES nativos, servidos tal cual desde GitHub Pages.
4. Librerías solo por CDN y con versión fijada. Pregunta antes de añadir una que no esté en el apartado 9.
5. Todos los textos de la interfaz, en español.
6. El token de Google vive solo en memoria. Nada de tokens en localStorage.
7. Todo HTML generado a partir de Markdown pasa por sanitización antes de pintarse.
8. Drive manda: ante cualquier duda de sincronización, gana lo que hay en Drive.
9. Al terminar cada versión, actualiza el apartado 13 (estado) de este fichero.

## 3. Almacenamiento en Google Drive

Estructura de la carpeta:

```text
Base de conocimiento/
  _indice.md                  generado por la web
  _catalogos.md               desde la versión 5
  proyectos/
    PRJ-<CLIENTE>-<TEMA>.md
  personal/
    PRJ-PERSONAL-<TEMA>.md
  sueltos/
    PRJ-SUELTOS-<AAAA-MM>.md  chats fuera de proyectos de Claude
  notas/
    NOTA-<AAAAMMDD-HHMMSS>.md notas rápidas pendientes de clasificar
  historico/
    PRJ-<...>_<AAAAMMDD-HHMM>.md versiones anteriores
```

1. La carpeta se elige en Ajustes, por nombre o pegando su identificador de Drive.
2. La carpeta de destino de cada MD sale del campo `tipo`: `personal` va a `personal/`; el resto, a `proyectos/`. Los MD cuyo id empieza por `PRJ-SUELTOS` van a `sueltos/`.
3. La web es la que escribe en Drive. No hace falta arrastrar ficheros a Drive ni usar Claude Code para el índice.
4. La firma ha autorizado guardar información de cliente en esta base.

## 4. Formato del MD de proyecto

Todos los MD siguen esta plantilla. La web depende de la cabecera para indexar y filtrar. La copia de trabajo está en `piloto/plantilla-proyecto.md`.

```markdown
---
id: PRJ-<CLIENTE>-<TEMA>
proyecto: <nombre corto>
cliente: <cliente o "personal">
proyecto_claude: <nombre del proyecto en Claude>
tipo: <implantación, AMS, propuesta, upgrade, interno o personal>
producto: <S/4HANA Public Cloud, ECC, BTP...>
modulos: [FI, CO]
normativa: [SII, Verifactu, DRC]
periodo: <AAAA-MM a AAAA-MM>
estado: <activo, cerrado o en propuesta>
confidencialidad: <cliente, interno o personal>
etiquetas: []
actualizado: <AAAA-MM-DD>
---

# <nombre del proyecto>

## Resumen
Cinco líneas como máximo: qué es, para quién y en qué punto está.

## Contexto y alcance

## Decisiones
| ID | Fecha | Decisión | Motivo |
|---|---|---|---|

## Conceptos reutilizables
| ID | Concepto | Explicación breve |
|---|---|---|

## Prompts
### PROMPT-<número> <título>
- Objetivo:
- Cuándo usarlo:
- Entradas que necesita:
- Prompt resumido (reutilizable, sin datos de cliente):

## Entregables
| Entregable | Formato | Estado |
|---|---|---|

## Lecciones aprendidas

## Pendientes a la fecha del export
```

Campos obligatorios de la cabecera: `id`, `proyecto`, `cliente`, `tipo`, `estado`, `actualizado`. El resto puede ir vacío.

Prefijos de ID de conceptos: NORM (normativa), TEC (técnico), PLA (planificación), MET (metodología), DEC (decisión), GLO (glosario). Los prompts usan PROMPT.

### Prompt de exportación

Se pega en un chat nuevo dentro de cada proyecto de Claude, porque Claude solo ve los chats del proyecto en el que está. La web lo muestra en Ajustes con un botón Copiar. Versión lista para pegar, con la plantilla ya incluida: `piloto/prompt-exportacion.txt`.

```text
Revisa todas las conversaciones de este proyecto, sus ficheros y sus instrucciones. Genera un único fichero Markdown con la plantilla de abajo, sin cambiar encabezados ni campos.

Reglas:
1. Solo hechos que aparezcan en las conversaciones o ficheros. Si un campo no tiene dato, déjalo vacío.
2. Distingue lo que decidí yo de lo que propusiste tú. Una propuesta tuya que no acepté no es una decisión.
3. Prompts: recoge los que usé varias veces o que dieron buen resultado. Resúmelos en versión reutilizable, sin nombres de cliente, importes ni datos personales.
4. Conceptos: solo los que sirvan en otro proyecto. Usa los prefijos NORM, TEC, PLA, MET, DEC y GLO.
5. Personas: solo por su rol (por ejemplo, gerente del cliente), nunca por nombre.
6. Español, frases cortas, sin relleno.
7. Entrégalo en un único bloque de código Markdown delimitado con cuatro acentos graves, listo para copiar y pegar.

[pegar aquí la plantilla]
```

### Formato de _indice.md

Lo genera la web en cada guardado. No se edita a mano.

1. Tabla de proyectos: id, proyecto, cliente, tipo, estado, periodo y resumen en una línea, ordenada por `actualizado` descendente.
2. Catálogo de prompts: id, título, objetivo y proyecto de origen.
3. Catálogo de conceptos: id, concepto y proyecto de origen.

### Formato de nota rápida

```markdown
---
id: NOTA-<AAAAMMDD-HHMMSS>
tipo_nota: <prompt, concepto, decisión o texto>
proyecto: <id del proyecto o vacío>
titulo: <título>
creada: <AAAA-MM-DDTHH:MM>
---

<texto>
```

## 5. Flujo de captura

1. Chat nuevo en el proyecto de Claude con el prompt de exportación y la plantilla.
2. Claude genera el MD en un bloque de código.
3. Enric lo revisa. Si mezcla propuestas de Claude con decisiones suyas, lo corrige en el mismo chat.
4. Lo pega (o sube el fichero) en la web.
5. La web lo guarda en Drive y actualiza `_indice.md` en la misma operación.
6. Queda disponible para consulta en escritorio y móvil.

## 6. Pantallas

| Pantalla | Escritorio | Móvil |
|---|---|---|
| Inicio | Buscador, recientes, favoritos y fecha de última sincronización | Igual, en una columna |
| Explorar | Tres paneles: filtros, lista y ficha. Tabla del índice con orden por columnas | Lista con filtros en hoja inferior |
| Ficha de proyecto | Resumen, decisiones, conceptos, prompts, entregables, lecciones y pendientes | Igual, con secciones plegables |
| Ficha de prompt | Objetivo, cuándo usarlo, entradas, texto y proyecto de origen | Igual |
| Catálogo de prompts | Todos los prompts, con búsqueda propia | Igual |
| Pegar MD | Cuadro de texto amplio con vista previa a la derecha | Cuadro de texto a pantalla completa, vista previa en pestaña |
| Subir fichero | Uno o varios .md a la vez | Un .md desde Archivos |
| Nota rápida | Formulario corto | Formulario corto, accesible desde la barra inferior |
| Editor | Editor de MD con vista previa al lado | Solo correcciones cortas |
| Ajustes | Cuenta de Google, carpeta, prompt de exportación, caché | Igual |

Navegación en móvil: barra inferior con Inicio, Explorar, Añadir y Ajustes.

## 7. Requisitos funcionales

### Consulta

- RF-01. Búsqueda de texto completo sobre todos los MD, tolerante a errores de escritura, con resultados mezclados (proyecto, decisión, concepto, prompt) y el tipo marcado.
- RF-02. Filtros por cliente, tipo, normativa, módulo y estado, leídos de la cabecera.
- RF-03. Ficha de proyecto con el MD renderizado y la fecha de última modificación en Drive.
- RF-04. Favoritos y recientes, guardados en el dispositivo.
- RF-05. Funcionamiento sin conexión con la última copia descargada.
- RF-06. Sincronización manual con botón y automática al abrir la web.

### Prompts

- RF-07. Botón Copiar en cada prompt.
- RF-08. Botón Abrir en Claude: copia al portapapeles el prompt precedido de la línea «Antes de responder, lee en Drive el fichero <id>.md» y abre `https://claude.ai/new`. Probar si el parámetro de texto precargado funciona; si funciona, usarlo además de copiar.

### Entrada de contenido

- RF-09. Pegar MD: lee la cabecera, muestra la vista previa y marca los campos obligatorios que falten, que se completan en un formulario antes de guardar.
- RF-10. Si el id ya existe, pregunta: reemplazar o guardar como versión nueva. En los dos casos, la versión anterior se copia a `historico/` con sello de fecha y hora.
- RF-11. Texto pegado sin cabecera: se trata como nota rápida y se piden tipo de nota y proyecto.
- RF-12. Subir fichero: mismas validaciones que Pegar MD, fichero a fichero. En escritorio, varios a la vez con resumen final de guardados y errores.
- RF-13. Nota rápida: formulario con título, proyecto, tipo de nota y texto. Se guarda en `notas/`.
- RF-14. Cada guardado actualiza `_indice.md` en la misma operación.

### Edición

- RF-15. Editor de MD con vista previa en escritorio. Al guardar, mismo tratamiento que RF-10.
- RF-16. Conflicto: si el fichero cambió en Drive desde que se abrió, avisar y ofrecer recargar o sobrescribir. Por defecto, gana el último guardado.

## 8. Cómo usa Claude la base

No es código de esta web, pero condiciona su diseño.

| Mecanismo | Qué hace |
|---|---|
| Conector de Google Drive en Claude | Claude busca y lee los MD de la carpeta desde cualquier chat |
| Prompt lanzado desde la web | Abre Claude con el prompt y la referencia al MD de origen (RF-08) |
| Skills | Los prompts más repetidos pasan a skill y se activan solos al pedir la tarea |

Criterio para pasar un prompt a skill: uso recurrente y resultado con formato fijo (Excel, tabla o documento).

## 9. Arquitectura técnica

| Pieza | Elección |
|---|---|
| Web | HTML, CSS y JavaScript con módulos ES, diseño de escritorio adaptado a móvil, tema claro y oscuro según el sistema |
| Autenticación | Google Identity Services, cliente de token en el navegador |
| Acceso a Drive | Drive API v3 con permiso de lectura y escritura. App personal en modo de pruebas de Google, sin verificación |
| Búsqueda | MiniSearch, índice construido en el navegador |
| Cabecera YAML | js-yaml |
| Markdown | marked para renderizar y DOMPurify para sanitizar |
| Caché sin conexión | IndexedDB |
| Publicación | GitHub Pages desde el repositorio de código |

Notas:

1. El repositorio solo contiene código. Puede ser público; si se quiere privado, GitHub Pages exige un plan de pago.
2. El identificador de cliente OAuth de Google va en un fichero de configuración. No es secreto, pero tampoco se reparte.
3. Usar el permiso amplio de Drive para poder leer ficheros creados fuera de la web (por ejemplo, desde el atajo de iOS). Si se limita al acceso por carpeta, comprobar antes que la web ve los ficheros que no ha creado ella.
4. js-yaml convierte `actualizado: 2026-11-01` en objeto Date. El parser debe cargar la cabecera con `JSON_SCHEMA` o normalizar las fechas a texto `AAAA-MM-DD` antes de indexar y de volver a escribir.

Estructura de código propuesta:

```text
index.html
config.js
css/app.css
js/main.js        arranque y rutas
js/auth.js        Google Identity Services
js/drive.js       lectura, escritura, histórico
js/parser.js      cabecera, secciones, prompts y conceptos
js/index.js       MiniSearch e _indice.md
js/cache.js       IndexedDB
js/ui/            una vista por pantalla
```

## 10. Mejoras

| N.º | Área | Mejora | Qué aporta | Esfuerzo | Versión |
|---|---|---|---|---|---|
| 1 | Captura | Nota rápida | Guardar un prompt, concepto o decisión suelta en segundos | Bajo | 3 |
| 2 | Captura | Atajo de iOS «Guardar en base» | Guardar desde la hoja de compartir de la app de Claude sin copiar y pegar; a validar | Bajo | 3 |
| 3 | Consulta | Inicio por proyecto activo | Pendientes y últimas decisiones de los proyectos en curso de un vistazo | Medio | 3 |
| 4 | Prompts | Variables en prompts | Campos como `{{cliente}}`, `{{periodo}}` o `{{perfiles}}` se rellenan en un formulario antes de lanzar | Medio | 4 |
| 5 | Prompts | Preparar contexto | Junta el MD del proyecto y el prompt en un solo texto para pegar en Claude | Bajo | 4 |
| 6 | Prompts | Contador de uso y valoración | Ordena por uso real y sirve para decidir qué pasa a skill | Bajo | 4 |
| 7 | Calidad | Aviso de duplicados | Detecta conceptos y prompts repetidos entre proyectos y propone fusionarlos | Medio | 4 |
| 8 | Captura | Exportación incremental con fusión | Reexportar un proyecto vivo solo con lo nuevo desde `actualizado` y fusionarlo en su MD | Alto | 5 |
| 9 | Captura | Migración de la BBDD en Excel | Los conceptos de BBDD_Conceptos_Enric.xlsx pasan a la base con sus IDs | Medio | 5 |
| 10 | Calidad | Avisos de fichas sin actualizar | Marca proyectos activos sin cambios en los últimos 90 días | Bajo | 5 |
| 11 | Calidad | Catálogos de etiquetas | Lista cerrada de clientes, módulos y normativa en `_catalogos.md` para que los filtros no se ensucien | Bajo | 5 |
| 12 | Calidad | Copia de seguridad | Descarga de toda la base en un fichero comprimido | Bajo | 5 |
| 13 | Consulta | Relaciones entre proyectos | Enlaces entre MD y conceptos compartidos, por ejemplo DRC en varios clientes | Alto | 6 |
| 14 | Consulta | Línea de decisiones por cliente | Todas las decisiones de un cliente en orden de fecha | Medio | 6 |
| 15 | Prompts | Generar SKILL.md | Convierte un prompt consolidado en skill lista para subir a Claude | Medio | 6 |

## 11. Decisiones abiertas

- [ ] Cuenta de Google donde vive la carpeta, que debe ser la misma que se conecte a Claude
- [ ] Activar el conector de Google Drive en Claude y comprobar que lee la carpeta y si permite crear ficheros
- [ ] Repositorio público o privado
- [ ] Cada cuánto se reexportan los proyectos activos
- [ ] Si la BBDD en Excel se migra a MD o se mantiene como fuente aparte
- [ ] Qué prompts pasan primero a skill
- [ ] Valor de `<CLIENTE>` en el id de los proyectos internos (por ejemplo, `PRJ-INTERNO-<TEMA>`)

## 12. Roadmap

### Versión 1. Piloto de exportación (sin código)

- Exportar tres proyectos: uno de cliente, uno interno y uno personal.
- Ajustar plantilla y prompt de exportación.
- Material de trabajo en `piloto/`.

Gate 1: plantilla validada con los tres proyectos.

### Versión 2. Web base (v0.1)

- Repositorio en GitHub, publicación en GitHub Pages y este CLAUDE.md en la raíz.
- RF-01, RF-03, RF-06, RF-07, RF-08, RF-09, RF-10, RF-11, RF-14.
- Ajustes: cuenta de Google, carpeta y prompt de exportación.

Criterios de aceptación:

1. Pegar uno de los MD del piloto lo guarda en la carpeta correcta y actualiza `_indice.md`.
2. Pegar el mismo id otra vez deja la versión anterior en `historico/`.
3. La búsqueda encuentra un prompt por una palabra de su objetivo.
4. Abrir en Claude deja el prompt en el portapapeles con la línea de contexto.

Gate 2: guardado en Drive desde el ordenador del trabajo. Si falla, la versión 3 se dedica a buscar otra vía de guardado antes de seguir con mejoras.

### Versión 3. Doble vista y exportación completa (v0.2)

- Vista móvil completa, RF-02, RF-04, RF-05, RF-12, RF-13, catálogo de prompts.
- Mejoras 1, 2 y 3.
- En paralelo, sin código: exportar el resto de proyectos de Claude y los chats sueltos.

### Versión 4. Prompts listos para lanzar (v0.3)

- RF-15, RF-16.
- Mejoras 4, 5, 6 y 7.

### Versión 5. Mantenimiento (v0.4)

- Mejoras 8, 9, 10, 11 y 12.

### Versión 6. Base conectada con Claude (v1.0)

- Mejoras 13, 14 y 15.
- Primeras skills generadas desde la base y en uso.

## 13. Estado

- Diseño funcional cerrado.
- Repositorio creado con esta especificación en la raíz.
- Versión 1 en curso. Kit del piloto en `piloto/`: plantilla, prompt de exportación listo para pegar y lista de control del Gate 1.
- Pendiente: exportar los tres proyectos del piloto, registrar los ajustes en `piloto/gate1.md` y cerrar el Gate 1. La versión 2 no empieza hasta entonces.
