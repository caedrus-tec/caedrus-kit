# COMPATIBILITY.md — dependencia externa, acoplamientos y defectos heredados

El kit no trae dentro el sistema de memoria que instala. La Fase 4.2 lo clona y
ejecuta su propio prompt de despliegue: es software de terceros, con su repositorio y
su ritmo de cambios. Este archivo declara de qué versión depende lo que se probó, por
dónde están soldadas las dos piezas, y qué defectos de ese sistema llegan al proyecto
fundado sin que el kit pueda evitarlo.

**Ninguna afirmación de este archivo aparece sin marca de origen.** El kit prefiere
decir *"esto solo consta por registro"* antes que dejar que una frase suene a
comprobación que nadie hizo.

## Marcas de origen

| Marca | Significado |
|---|---|
| `verificado@b95eda9 (fecha)` | contrastado contra un checkout del sistema externo en ese commit, en esa fecha, **leyendo su texto**. No se ejecutó el despliegue desde ese checkout: lo verificado es qué dice el sistema externo que hace, no su comportamiento al correr. Ver `LIMITS.md`, *Estado de la verificación de la dependencia externa* |
| `declarado-por-registro (1ª validación, 2026-08-19/20)` | consta en el acta de la primera validación de campo; **no reverificado** |
| `no-verificable-desde-el-kit` | depende del harness o del repositorio del consumidor; lleva enlace a `LIMITS.md` |

Tres reglas de uso. Ninguna fila puede quedar sin marca. Una fila
`declarado-por-registro` **nunca** se redacta en presente de indicativo (*"el
despliegue copia…"*) sino como registro fechado (*"según el acta de la primera
validación, el despliegue copiaba…"*). Y una fila `verificado@` describe **lo que el
texto del sistema externo instruye**, no lo que ocurre al ejecutarlo: donde la
diferencia importe, la fila lo dice. Es la diferencia entre informar y prometer.

En `v1.0.0` ninguna fila lleva la marca `declarado-por-registro`: la verificación
contra el clon se ejecutó y las ocho afirmaciones dependientes se pudieron
contrastar una a una.

## La dependencia

- **Repositorio:** `iv0nis/memsys3`.
- **Versión probada:** **v0.29.1**, commit `b95eda9`. Es la única contra la que se ha
  contrastado nada de lo que sigue.
- **Cómo entra:** la Fase 4.2 lo clona en `memsys3_temp/` y lee y ejecuta su prompt
  `memsys3_templates/prompts/deploy.md` paso a paso. El kit no reimplementa ninguna
  parte del despliegue: lo conduce.
- **El clonado fija versión, y el runbook lo hace por ti.** La Fase 4.2 clona y a
  continuación ejecuta `git -C memsys3_temp checkout b95eda9`, de modo que lo que se
  instala es lo que aquí se declara probado. Si decides saltarte ese `checkout` —o si
  ese commit deja de existir— tomas la rama por defecto, y en cuanto el sistema
  externo avance desde `b95eda9` las ocho filas de la tabla siguiente dejan de estar
  respaldadas.

## Qué depende del sistema externo y qué se rompe si cambia

Las ocho afirmaciones del kit que no se sostienen solas. Todas se contrastaron
contra el checkout de `b95eda9`.

| # | Lo comprobado en `b95eda9` | Qué se rompe si el upstream cambia | Marca |
|---|---|---|---|
| 1 | El Paso 2 copia el distribuible **por enumeración**, y de la carpeta de prompts su texto copia el conjunto entero con un glob (`prompts/*.md`): los 14 prompts que hay en `b95eda9` están enumerados para entrar en el proyecto fundado | Si deja de copiar la carpeta de prompts entera, la lista de `## Comandos útiles` del `README.md` generado deja de estar respaldada | `verificado@b95eda9 (2026-08-21)` |
| 2 | El Paso 8 comprueba la exclusión de la memoria buscando el prefijo `memsys3` **a principio de línea** en `.gitignore` | Si el ancla deja de exigir inicio de línea, el bloque de andamiaje del kit —anclado con `/`— pasaría a disparar un aviso falso | `verificado@b95eda9 (2026-08-21)` |
| 3 | El Paso 9 crea `MEMORY.md` de forma **opcional**, condicionada a lo decidido en el briefing, y declara su degradación si se omite | Si pasara a crearlo siempre, el inventario de la Fase 6 marcaría como residuo un archivo que el despliegue instala | `verificado@b95eda9 (2026-08-21)` |
| 4 | El Paso 10 **no sobrescribe** un `AGENTS.md` existente: avisa y deja el merge del invariante al usuario | Si pasara a sobrescribir, la rama condicional del punto 4 de la Fase 4.2 sería falsa y el documento previo del usuario se perdería | `verificado@b95eda9 (2026-08-21)` |
| 5 | El `AGENTS.md` del distribuible tiene 11 líneas: el título literal del invariante, los tres párrafos idénticos al núcleo congelado `A2` (diff 0) y un marcador de versión propio en la última. El Paso 10 copia el archivo entero, marcador incluido | Si ese texto cambia, el `AGENTS.md` que produce la Fase 5.2 y el que instalaría el despliegue dejan de decir lo mismo | `verificado@b95eda9 (2026-08-21)` |
| 6 | `prompts/endSession.md` existe, y su texto instruye escribir en `memory/full/sessions.yaml` y `memory/project-status.yaml`. Que lo escriba al correr no se ha comprobado: lo que se leyó es el prompt | Acoplamiento 4: el paso 5 de la Fase 6 cae a su modo degradado | `verificado@b95eda9 (2026-08-21)` |
| 7 | `memory/templates/sessions-template.yaml` y `memory/templates/project-status-template.yaml` existen y el Paso 2 los enumera para instalarlos | El modo degradado del paso 5 de la Fase 6 se queda sin los schemas en los que se apoya | `verificado@b95eda9 (2026-08-21)` |
| 8 | Existen en el distribuible y el Paso 2 los enumera: `memory/memory.yaml` (14 líneas) y `memory/context.yaml` (31), copiados por nombre; `memory/full/adr.yaml` (5) por el glob de su carpeta; y `memory/templates/memory-template.yaml` (54) por el glob `*.yaml`. Además, el **Paso 12** compila `context.yaml` en línea al terminar el despliegue y exige que el archivo no quede vacío | Cuatro rutas que el kit nombra dejarían de existir en el proyecto fundado: la del **invariante congelado `A2`** (`memory/memory.yaml`), las dos de `templates/README.template.md` y `templates/AGENTS.template.md`, y la que comprueba el `test -s` del paso 5 de la Fase 4.2 | `verificado@b95eda9 (2026-08-24)` |

Las siete primeras se recontrastaron contra el mismo checkout el 2026-08-24, sin
cambios; la octava se añadió ese mismo día, contra ese mismo checkout.

## Los cuatro acoplamientos

| # | Acoplamiento | Qué se rompe si cambia | Marca |
|---|---|---|---|
| 1 | El título literal `## Invariante de memoria agnóstica (ADR-027)` | La propagación del invariante al proyecto fundado: una sección con otro título deja de ser reconocible como esa sección | `verificado@b95eda9 (2026-08-24)` |
| 2 | El schema de `deploy-config-template.yaml`: el Paso 3 exige rellenos `proyecto.nombre`, `proyecto.dominio`, `proyecto.objetivo`, `proyecto.audiencia`, `proyecto.fase`, `proyecto.idioma`, `gitignore_memsys3` y `memory_bridge` | El modo declarativo: el despliegue vuelve al briefing conversacional, y la Fase 4.1 acaba preguntando por claves que ya no existen | `verificado@b95eda9 (2026-08-24)` |
| 3 | La sintaxis de import del harness (`@AGENTS.md`) y los nombres de los dos archivos del puente (`AGENTS.md` y `CLAUDE.md`), que fija el harness y no este sistema externo | El puente de la Fase 5.3, con **fallo silencioso**: un `CLAUDE.md` cuya sintaxis el harness ya no reconoce no da ningún error; el agente trabaja sin las reglas y nada lo anuncia. Ver `LIMITS.md`, entradas B2 y D7 | `no-verificable-desde-el-kit` |
| 4 | `prompts/endSession.md` como mecanismo del cierre de memoria del paso 5 de la Fase 6 | Ese paso cae a su modo degradado: cumplir el mismo contrato de contenido leyendo los schemas que el despliegue dejó instalados en el propio proyecto | `verificado@b95eda9 (2026-08-24)` |

## Defectos heredados

Son defectos del sistema externo, no del kit. El kit no es dueño de ese repositorio y
no los corrige: los declara, porque llegan al proyecto fundado.

| Id | Qué es | Cómo llega al proyecto fundado | Marca |
|---|---|---|---|
| U1 | El Paso 2 copia por enumeración y omite piezas del distribuible: el `README.md` de la carpeta de plantillas, `blocked_files_log.md`, `docs/reference.md`, `backlog/archive/` y, dentro de las plantillas de memoria, `operations-template.log` —de esa carpeta solo copia `*.yaml` y `*.md`— | El proyecto queda sin la documentación de referencia del sistema de memoria. Ninguna de las seis fases del kit usa esos archivos, y el kit no los suple | `verificado@b95eda9 (2026-08-24)` |
| U2 | Falso positivo del Paso 8: un `.gitignore` con cualquier línea que empiece por `memsys3` le hace avisar de que la memoria quedó excluida de git, aunque no lo esté | **Cerrado por el lado del kit**: el bloque de andamiaje ancla sus tres rutas con `/`, así que ninguna línea que el kit escribe empieza por ese prefijo y el aviso no se dispara. Sin la barra inicial sí se dispararía | `verificado@b95eda9 (2026-08-24)` |
| U3 | `prompts/github.md` empuja a `master` en dos puntos, sin comprobar antes cuál es la rama del repositorio | El prompt viaja entero al proyecto fundado. Si su rama es `main`, esos dos comandos fallan. Ninguna fase del kit lo ejecuta | `verificado@b95eda9 (2026-08-24)` |
| U4 | `prompts/endSession.md` guarda un identificador de agente **fuera del proyecto**, en una ruta del directorio personal del usuario cuyo nombre incluye el de un harness concreto; y propone la atribución de la sesión con ese mismo nombre de harness escrito a mano (`"Claude (Session [Título])"`) | El cierre de memoria deja rastro fuera del repositorio, y puede firmar la sesión con un harness que no es el que la ejecutó | `verificado@b95eda9 (2026-08-21)` |

Dos precisiones que la tabla no debe dejar implícitas. **U2 cerrado por el lado del
kit no es U2 corregido:** el ancla sigue en el sistema externo, y cualquier línea que
el consumidor escriba a mano en su `.gitignore` puede volver a dispararlo. Y de
**U4**, el paso 5 de la Fase 6 instruye sustituir la atribución cuando no coincide
con el ejecutor; la escritura fuera del proyecto no la cubre nada del kit, porque
ocurre dentro del prompt del sistema externo.

## Precisión sobre el comentario `NO EDITAR` del invariante

El núcleo congelado `A2` incluye un comentario que afirma que el despliegue *"la
busca por este título"*. El mecanismo real es más débil: en el sistema externo esa
línea del Paso 10 es un **comentario dentro de un bloque de shell**, no una
comprobación que falle sola; lo que decide ese bloque es la existencia del archivo,
no su título. `verificado@b95eda9 (2026-08-24)`.

**La única comprobación automática por título la hace el runbook de este kit**, dos
veces: al cerrar la Fase 5 y en la verificación final de la Fase 6, con
`grep -c "Invariante de memoria agnóstica (ADR-027)" AGENTS.md   # debe dar 1`.

El comentario **se conserva tal cual**. Es parte de un literal congelado —ver
`LITERALS.md`, entrada `A2`, *Desviaciones conocidas*— y corregirlo rompería el
literal para ganar una exactitud que ya está registrada aquí. Quien venga a
"limpiarlo" debe leer antes esta sección.

Detalle asociado: el `AGENTS.md` del distribuible externo lleva su propio marcador de
versión en la última línea y el Paso 10 lo copia con él. El kit no lo arrastra: su
`AGENTS.md` se genera en la Fase 5.2 desde `templates/AGENTS.template.md`, que lleva
el marcador del kit y ninguno ajeno. `verificado@b95eda9 (2026-08-24)`.

## Precisión sobre el ahorro de contexto del comentario del puente

El núcleo congelado `A1.p2` dice, del comentario HTML que puede acompañar al import
del puente, que el harness *"lo elimina antes de inyectar el archivo en contexto, así
que no consume tokens"*. **Ese comportamiento no está verificado, y este kit no puede
verificarlo:** no es texto de una dependencia que se lea en un commit congelado, sino
conducta interna de un programa en una versión concreta, que además puede cambiar sin
aviso. Misma categoría que el acoplamiento 3. `no-verificable-desde-el-kit`.

Qué significa en la práctica: **nada de lo que el runbook manda hacer depende de que
sea cierto.** El comentario es opcional —el paso 1 dice *"puedes dejar"*— y el puente
funciona igual con él o sin él. Si el harness no lo descartara, el único efecto sería
que ese comentario ocupa contexto como cualquier otro texto del archivo. Si el ahorro
te importa, no escribas el comentario.

El texto **se conserva tal cual**: es parte de un literal congelado —ver `LITERALS.md`,
entrada `A1.p2`, *Desviaciones conocidas*— y reescribirlo rompería la propiedad que
hace comprobable la cadena de integridad. Quien venga a "limpiarlo" debe leer antes
esta sección.

## Por qué `## Comandos útiles` enumera cuatro prompts y no más

`templates/README.template.md` ofrece cuatro: `newSession.md`, `endSession.md`,
`compile-context.md` y `adr.md`. La razón es la fila 1 de la tabla de dependencias:
el Paso 2 copia la carpeta de prompts entera, de modo que los 14 prompts —esos cuatro
incluidos— existen en cualquier proyecto fundado con esta dependencia en `b95eda9`.
Es lo que hace que la lista sea defendible: no ofrece nada que el despliegue no
instale. `verificado@b95eda9 (2026-08-21)`.

Los demás quedan fuera por criterio, no por ausencia: unos gobiernan el propio
despliegue o la migración, otros sirven flujos que el proyecto recién fundado todavía
no ha decidido, y uno de ellos arrastra U3.

La lista se declara **subconjunto conservador y no inventario exhaustivo** por dos
motivos, y así lo dice también la plantilla en su propio texto: la copia por
enumeración del Paso 2 puede cambiar sin aviso —U1 muestra que ya omite piezas—, y el
proyecto fundado añadirá sus propios comandos en el slot que la plantilla le reserva.

## Cuando el sistema externo avance

Nada de esto se detecta solo. Quien actualice la dependencia tiene que recontrastar
las ocho filas y los cuatro acoplamientos contra el commit nuevo y escribir la fecha
nueva en cada marca. Una afirmación cuya marca no se pueda renovar no se queda como
estaba: baja a `declarado-por-registro` con la fecha de su última comprobación, o se
muda a `LIMITS.md` si deja de ser comprobable. El cambio de versión probada se anota
en `CHANGELOG.md`.
