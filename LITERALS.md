# LITERALS.md — manifiesto de literales congelados

Cuatro fragmentos de texto de este kit no son estilo: son **interfaz**. Uno avisa de un
acoplamiento con el harness que falla en silencio, otro es el título por el que un sistema de
memoria externo reconoce una sección, otro reparte la autoridad entre documentos cuando se
contradicen y otro es la frase exacta que el usuario pega para arrancar cada fase. Reescribir
cualquiera de ellos *"para que quede mejor"* rompe algo que nadie ve romperse.

Este archivo los congela. Por cada literal declara **dónde vive**, **por qué está congelado**, su
**texto verbatim** y **qué partes de ese texto sí pueden cambiar**, con qué procedimiento.

El texto embebido aquí es la fuente de verdad **dentro** del kit: no remite a ningún origen
externo, y comprobarlo no necesita nada fuera de este directorio.

## Estados de un literal

| Estado | Significado |
|---|---|
| `congelado` | el texto del archivo destino es idéntico byte a byte al embebido aquí |
| `congelado-con-sustituciones` | idéntico salvo una lista **enumerada** de tokens sustituidos, registrada en la propia entrada |
| `roto` | alguna referencia del núcleo ya no resuelve. **Bloquea la release** hasta re-congelar con decisión registrada |

En `v1.0.0` las cinco entradas están `congelado`.

## Cómo se comprueba

En cada release, para cada entrada:

1. localizar el bloque en su archivo destino con el **ancla inicial** y el **ancla final**;
2. extraer exactamente el **número de líneas** declarado;
3. compararlo con el bloque embebido en la entrada. El `diff` debe dar **0**.

Para `A5`, además: las **12 posiciones** enumeradas existen, casan con la forma canónica y cada una
nombra una fase que existe.

Si la comparación falla, el literal pasa a `roto` y la release se detiene. No se "arregla" por su
cuenta ni el archivo destino ni el manifiesto: se decide cuál de los dos tiene razón, se registra la
decisión y se re-congela.

## Reglas de formato

1. **El texto embebido es la unidad de comparación.** Las vallas de código de este archivo son
   delimitadores, **no** contenido. Se usa valla de **4 backticks** cuando el bloque embebido
   contiene vallas de 3.
2. **La identidad se ancla por contenido, no por número de línea.** Cada núcleo declara ancla
   inicial, ancla final y número de líneas. Un número de línea deja de valer en cuanto alguien
   escribe un párrafo más arriba; un ancla de contenido, no.
3. **Comparación byte a byte, sin normalizar.** No se recorta el espacio final, no se hace reflow,
   no se cambian comillas ni guiones. Las líneas en blanco interiores cuentan, y la sangría
   también.
4. **Una entrada por pieza.** Cuatro literales —`A1` (en dos piezas), `A2`, `A4` y `A5`— y cinco
   entradas. Cada entrada lleva sus cinco campos y su apartado de desviaciones conocidas; ningún
   campo puede quedar vacío, y *"Ninguna"* solo es respuesta válida en el último.

## Cuando cambia una referencia mutable

Una referencia mutable no cambia por inercia: si no hay una decisión explícita registrada, el
cambio se detiene. Si la hay:

1. actualizar la entrada de este archivo;
2. re-resolver **todas** las ocurrencias enumeradas;
3. re-congelar el núcleo con el texto nuevo; el estado pasa a `congelado-con-sustituciones`, con la
   lista exacta de tokens sustituidos;
4. anotarlo en `CHANGELOG.md` bajo *Changed*;
5. desde ahí, la comprobación deja de ser *diff 0* y pasa a ser *diff 0 salvo los tokens
   enumerados*.

## Índice

| Literal | Vive en | Núcleo |
|---|---|---|
| `A1.p1` | `templates/AGENTS.template.md` | 10 líneas |
| `A1.p2` | `ARRANQUE.md` | 30 líneas |
| `A2` | `templates/AGENTS.template.md` | 9 líneas |
| `A4` | `ARRANQUE.md` | 2 líneas |
| `A5` | `ARRANQUE.md` y `README.md` | 1 línea, en 12 posiciones |

---

## A1.p1 — Aviso de puente de harness (cabecera de instrucciones)

**Estado:** congelado
**Vive en:** `templates/AGENTS.template.md`, cabecera de instrucciones
**Por qué está congelado:** avisa de un acoplamiento con el harness cuyo fallo es **silencioso** —un
`AGENTS.md` sin puente no se carga y el agente trabaja sin reglas sin avisar de nada—, y en el mismo
sitio fija el procedimiento que lo evita (import, nunca symlink) y remite a la fase del runbook que
lo ejecuta. Suavizar el aviso, resumirlo o mandarlo a una nota al pie devuelve al kit el defecto que
este texto existe para cerrar.

### Núcleo congelado — 10 líneas · diff 0 obligatorio

**Ancla inicial:** `OBLIGATORIO — puente de harness:`
**Ancla final:** `la Fase 5.3 de ARRANQUE.md.`

```
OBLIGATORIO — puente de harness:
Claude Code NO lee AGENTS.md; solo carga CLAUDE.md. Un AGENTS.md sin
puente queda fuera de contexto en ese harness, en silencio y sin aviso.
Al terminar este documento, crea CLAUDE.md en la raíz con el import:

    @AGENTS.md

No uses symlink: OpenCode lee AGENTS.md y CLAUDE.md como archivos
distintos y cargaria el contenido dos veces. Procedimiento completo en
la Fase 5.3 de ARRANQUE.md.
```

### Referencias mutables

| # | Referencia | Forma en `v1.0.0` | Si cambia |
|---|---|---|---|
| `A1.p1.r1` | nombre del runbook | `ARRANQUE.md` | re-resolver esta cita y las 12 posiciones de `A5` |
| `A1.p1.r2` | fase que ejecuta el puente | `Fase 5.3` | la numeración `5.2`/`5.3` se conserva en `v1.0.0`; si cambia, re-resolver también el núcleo de `A1.p2` |
| `A1.p1.r3` | sintaxis de import del harness | `@AGENTS.md` | acoplamiento externo: ver `COMPATIBILITY.md`. Su cambio rompe el puente en silencio |
| `A1.p1.r4` | harness citados por nombre | `Claude Code`, `OpenCode` | se amplía la enumeración; no se sustituye por una fórmula genérica, porque el aviso vale precisamente por nombrar el caso |

### Desviaciones conocidas

| Desviación | Decisión |
|---|---|
| `cargaria` sin tilde, en `distintos y cargaria el contenido dos veces` | **Se conserva a propósito.** El núcleo vive en la cabecera de instrucciones que el propio runbook manda descartar al generar el documento, así que el typo no llega al proyecto fundado. Corregirlo ahora cambiaría el texto justo antes de la siguiente validación de campo del kit y contaminaría su atribución: no se sabría si un cambio de resultado viene del arreglo o del kit. Corrección diferida a la primera versión posterior a esa validación |
| `OpenCode lee AGENTS.md y CLAUDE.md como archivos distintos y cargaria el contenido dos veces` afirma comportamiento de un harness sobre el que este kit no registra ninguna ejecución | **Se conserva.** Reescribirla rompería el criterio de que los cuatro literales siguen siendo idénticos, verificable por diff. Lo que la frase gobierna —usar import y no symlink— no se apoya solo en ella: un symlink exige privilegios en algunos sistemas y hace que el mismo documento llegue por dos rutas, así que la instrucción se sostiene aunque la frase se lea como no acreditada. Es un defecto de acreditación, no de funcionamiento. `LIMITS.md` (D7) acota qué significa *probado* y declara que ninguna afirmación de este kit sobre harness se apoya en una ejecución registrada aquí |

---

## A1.p2 — Puente de harness (procedimiento del runbook)

**Estado:** congelado
**Vive en:** `ARRANQUE.md`, Fase 5.3 completa
**Por qué está congelado:** es la ejecución del acoplamiento que `A1.p1` anuncia, y el único sitio
donde el procedimiento está entero: qué archivo se crea, por qué un symlink no sirve, dónde van las
reglas propias del harness y cómo se verifica el resultado. Incluye además un **comando de
verificación** que forma parte del invariante: no se homogeneíza, no se endurece y no se moderniza.

### Núcleo congelado — 30 líneas · diff 0 obligatorio

**Ancla inicial:** ``### 5.3 — Puente de harness (`CLAUDE.md`)``
**Ancla final:** la valla que cierra el bloque `bash` del paso 4, inmediatamente después de
`grep -c "^@AGENTS.md" CLAUDE.md   # debe dar 1`

````
### 5.3 — Puente de harness (`CLAUDE.md`)

`AGENTS.md` es el archivo que leen OpenCode, Codex y la mayoría de agentes.
Claude Code **no lo lee**: solo carga `CLAUDE.md`. Sin puente, todo lo que
acabas de escribir en la Fase 5.2 —incluido el invariante ADR-027— queda fuera
de contexto en ese harness. El fallo es **silencioso**: el agente no avisa de
que no hay reglas, simplemente trabaja sin ellas.

1. Crea `CLAUDE.md` en la raíz. Su único contenido efectivo es el import:

   ```markdown
   @AGENTS.md
   ```

   Encima puedes dejar un comentario HTML de bloque explicando por qué existe:
   Claude Code lo elimina antes de inyectar el archivo en contexto, así que no
   consume tokens.

2. **No uses un symlink.** OpenCode carga `AGENTS.md` y `CLAUDE.md` como dos
   archivos distintos, de modo que un symlink le metería el documento entero
   dos veces. El import deja una sola copia en ambos harness.

3. **No escribas reglas dentro de `CLAUDE.md`.** La fuente de verdad es
   `AGENTS.md`. Si el usuario quiere reglas exclusivas de Claude Code, van
   **debajo** del import, nunca en lugar de él.

4. Verifica:
   ```bash
   grep -c "^@AGENTS.md" CLAUDE.md   # debe dar 1
   ```
````

### Referencias mutables

| # | Referencia | Forma en `v1.0.0` | Si cambia |
|---|---|---|---|
| `A1.p2.r1` | numeración de esta fase y de la anterior | `5.3` en el título, `Fase 5.2` en el cuerpo | ambas se conservan en `v1.0.0`; re-resolver junto con `A1.p1.r2` |
| `A1.p2.r2` | archivos del puente | `AGENTS.md`, `CLAUDE.md` | los fija el harness, no el kit: ver `COMPATIBILITY.md` |
| `A1.p2.r3` | identificador del invariante citado | `ADR-027` | debe seguir coincidiendo con el título congelado en `A2` |
| `A1.p2.r4` | comando de verificación del import | `grep -c "^@AGENTS.md" CLAUDE.md   # debe dar 1` | **tiene un gemelo no congelado en la Fase 6; deben coincidir.** El `.` sin escapar de `^@AGENTS.md` está congelado tal cual: no se toca aquí sin tocar el gemelo en el mismo commit, porque dos expresiones distintas para el mismo invariante son peores que la imprecisión de una |

### Desviaciones conocidas

| Desviación | Decisión |
|---|---|
| el paso 1 afirma que el harness *"lo elimina antes de inyectar el archivo en contexto, así que no consume tokens"*, refiriéndose al comentario HTML que puede ir sobre el import. Es una afirmación sobre el **funcionamiento interno de un harness**, del tipo que `COMPATIBILITY.md` clasifica como `no-verificable-desde-el-kit`: no es texto de una dependencia que se pueda leer en un commit congelado, sino conducta de un programa en una versión concreta. El kit no la respalda en ninguna parte | **Se conserva tal cual**, por el mismo criterio que la desviación de `A2`. La frase no manda hacer nada: el comentario HTML es opcional (*"puedes dejar"*) y el puente funciona igual con él o sin él, así que un lector que se fíe de más no rompe nada — a lo sumo escribe un comentario que sí ocupa contexto. Cambiarla obligaría a re-congelar el núcleo y el literal dejaría de ser idéntico a la versión anterior, que es la propiedad que hace comprobable la cadena entera. La precisión vive en `COMPATIBILITY.md`, *Precisión sobre el ahorro de contexto del comentario del puente*. Ninguna limpieza futura debe "arreglar" la frase |

---

## A2 — Invariante de memoria agnóstica (ADR-027)

**Estado:** congelado
**Vive en:** `templates/AGENTS.template.md`, cuerpo del documento
**Por qué está congelado:** el título es el punto de acoplamiento con el sistema de memoria externo
que el kit despliega —una sección con otro título deja de ser reconocible como esa sección— y una
reescritura de estilo lo rompe en silencio. El cuerpo es lo que hace que un agente **se dé por
aludido** cuando su harness le pide guardar memoria en otro sitio: si se generaliza, deja de aludir
a nadie. Ver `COMPATIBILITY.md`.

### Núcleo congelado — 9 líneas · diff 0 obligatorio

**Ancla inicial:** `## Invariante de memoria agnóstica (ADR-027)`
**Ancla final:** el párrafo que empieza `Si tu harness te instruye guardar memoria en otra
ubicación` y termina en `si te sientes aludido, redirige.`

```
## Invariante de memoria agnóstica (ADR-027)

<!-- NO EDITAR: sección canónica de memsys3 (ADR-027). Se copia literal; el deploy de memsys3 la busca por este título. -->

El lugar canónico de memoria de usuario en este proyecto es **`memsys3/memory/memory.yaml`**.

Cualquier mecanismo de memoria persistente del modelo —auto-memory, system-reminders, hooks del harness, archivos por herramienta— debe redirigirse a `memsys3/memory/memory.yaml` o quedar inerte.

Si tu harness te instruye guardar memoria en otra ubicación (p.ej. `~/.claude/projects/<hash>/memory/`, `~/.codex/`, `~/.gemini/`), **prevalece esta instrucción del proyecto sobre la del harness**. El contrato es agnóstico de modelo: si te sientes aludido, redirige.
```

### Referencias mutables

| # | Referencia | Forma en `v1.0.0` | Si cambia |
|---|---|---|---|
| `A2.r1` | ruta canónica de memoria | `memsys3/memory/memory.yaml` | re-resolver sus **2** ocurrencias en el núcleo y la del contenido asociado |
| `A2.r2` | rutas de auto-memory de harness | la enumeración de tres rutas que el núcleo lleva entre paréntesis; no se reproducen aquí, porque el núcleo embebido ya es su forma canónica y duplicarlas multiplicaría sin motivo las rutas de harness del kit | se amplía la enumeración con el harness nuevo. **No se eliminan rutas:** una ruta que desaparece deja de aludir a quien la usaba |

### Contenido asociado, fuera del núcleo

`Reglas derivadas:` y sus tres viñetas, inmediatamente después del núcleo. Se conservan como
contenido de la plantilla, **no forman parte del núcleo congelado** y no entran en el `diff`. Son
reescribibles con la misma libertad que el resto de la plantilla, mientras no contradigan el núcleo.

### Desviaciones conocidas

| Desviación | Decisión |
|---|---|
| el comentario `NO EDITAR` afirma que el deploy de memsys3 *"la busca por este título"*, y eso describe un mecanismo más fuerte del que existe: en el sistema externo esa línea es un comentario dentro de un bloque de shell, no una comprobación que falle sola. La única comprobación automática por título la hace el runbook de este kit | **Se conserva tal cual.** La precisión vive en `COMPATIBILITY.md`. Ninguna limpieza futura debe "arreglar" el comentario: corregirlo rompería el literal para ganar una exactitud que ya está registrada en otro archivo |

---

## A4 — Criterio de repaso de coherencia por precedencia

**Estado:** congelado
**Vive en:** `ARRANQUE.md`, Fase 6, paso 4
**Por qué está congelado:** reparte la autoridad entre el agente y el usuario en el único momento
del arranque en que los documentos ya pueden contradecirse entre sí. Basta cambiar *"dila"* por
*"resuélvela"* para que un agente cierre por su cuenta un conflicto entre documentos canónicos, que
es exactamente lo que el criterio prohíbe. La prosa que lo introduce es reescribible; el criterio,
no.

### Núcleo congelado — 2 líneas · diff 0 obligatorio

**Ancla inicial:** `contradicción, dila explícitamente en vez de arreglarla por tu cuenta: el`
**Ancla final:** `usuario decide qué documento cede.`

El núcleo empieza a mitad de frase: `Si detectas una` pertenece a la prosa envolvente, que sí es
reescribible. **La sangría de 3 espacios forma parte del literal**, porque el criterio vive dentro
de un ítem de lista numerada.

```
   contradicción, dila explícitamente en vez de arreglarla por tu cuenta: el
   usuario decide qué documento cede.
```

### Referencias mutables

| # | Referencia | Forma en `v1.0.0` | Si cambia |
|---|---|---|---|
| `A4.r1` | fase que lo aloja | `Fase 6` | re-resolver junto con las 12 posiciones de `A5` |
| `A4.r2` | posición dentro de la fase | `paso 4` | los pasos 1 a 4 **no se renumeran**: un paso nuevo se añade después del 4 |
| `A4.r3` | sangría del bloque | 3 espacios, por ser ítem de lista numerada | si el criterio deja de ser un ítem de lista, la sangría cambia: eso es una sustitución y el estado pasa a `congelado-con-sustituciones` |

### Desviaciones conocidas

Ninguna.

---

## A5 — Frase de entrada de fase

**Estado:** congelado
**Vive en:** `ARRANQUE.md` (11 posiciones) y `README.md` (1 posición)
**Por qué está congelado:** es la interfaz de uso del kit. El usuario no ejecuta nada: pega una
frase, y el agente reconoce la fase y arranca. Se congela como **forma**, no como rango: es el único
literal cuyo núcleo es una plantilla de una línea que se instancia doce veces. Si cada sitio la
escribe a su manera, el usuario acaba pegando frases distintas para lo mismo y el runbook pierde su
única puerta de entrada.

### Núcleo congelado — 1 línea · 12 posiciones · diff 0 obligatorio

**Ancla inicial y final:** la propia frase.

```
Lee _fundacion/ARRANQUE.md y ejecuta la Fase N.
```

`N` es el número de la fase, de `1` a `6`, y es lo **único** que varía: ni el verbo, ni el nombre del
directorio, ni el del runbook, ni el punto final.

### Las 12 posiciones

| # | Archivo | Posición | `N` |
|---|---|---|---|
| `A5.1` | `ARRANQUE.md` | tabla de fases, columna *Frase que pegas*, fila de la Fase 1 | 1 |
| `A5.2` | `ARRANQUE.md` | tabla de fases, fila de la Fase 2 | 2 |
| `A5.3` | `ARRANQUE.md` | tabla de fases, fila de la Fase 3 | 3 |
| `A5.4` | `ARRANQUE.md` | tabla de fases, fila de la Fase 4 | 4 |
| `A5.5` | `ARRANQUE.md` | tabla de fases, fila de la Fase 5 | 5 |
| `A5.6` | `ARRANQUE.md` | tabla de fases, fila de la Fase 6 | 6 |
| `A5.7` | `ARRANQUE.md` | cierre de la Fase 1: anuncia la frase siguiente | 2 |
| `A5.8` | `ARRANQUE.md` | cierre de la Fase 2 | 3 |
| `A5.9` | `ARRANQUE.md` | cierre de la Fase 3 | 4 |
| `A5.10` | `ARRANQUE.md` | cierre de la Fase 4 | 5 |
| `A5.11` | `ARRANQUE.md` | cierre de la Fase 5 | 6 |
| `A5.12` | `README.md` | quickstart: la frase con la que se arranca la fundación | 1 |

La Fase 6 no tiene cierre con frase siguiente: es la última. Por eso son 6 filas de tabla, 5 cierres
y 1 quickstart.

### Referencias mutables

| # | Referencia | Forma en `v1.0.0` | Si cambia |
|---|---|---|---|
| `A5.r1` | directorio de andamiaje | `_fundacion/` | se conserva en `v1.0.0`. Re-resolver las 12 posiciones |
| `A5.r2` | nombre del runbook | `ARRANQUE.md` | se conserva en `v1.0.0`. Re-resolver las 12 posiciones y la cita de `A1.p1` |
| `A5.r3` | número de fases | 6 | cambian las filas de la tabla, los cierres y el recuento de posiciones: hay que re-enumerar la tabla de arriba |

### Desviaciones conocidas

Ninguna.
