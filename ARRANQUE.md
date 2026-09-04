# ARRANQUE — fundación guiada de un proyecto nuevo

Este archivo conduce la fundación completa de un proyecto: de un repositorio
vacío a una base canónica, memoria operativa y documentos alineados, lista para
empezar a desarrollar.

**Cómo se usa:** hay 6 fases. En cada una abres una sesión nueva con tu agente
(OpenCode, Claude Code o el que uses) y le pegas **una sola frase**. El agente
lee este archivo, ejecuta la fase, te va preguntando lo que necesite y la cierra
con un commit.

**Antes de empezar, lee `LIMITS.md`.** De los agentes que este kit dice soportar,
solo uno está probado, y el runbook no repite esa reserva en cada fase. `LIMITS.md`
dice qué se ha ejecutado de verdad y qué no; `COMPATIBILITY.md`, de qué versión del
sistema de memoria externo depende lo probado y qué defectos suyos llegan a tu
proyecto. Los dos están en la raíz del kit, junto a este archivo. Si algo de aquí te
suena a garantía, ahí está su alcance real.

| Fase | Qué produce | Frase que pegas |
|------|-------------|-----------------|
| 1 | `docs/blueprint/VisionScope.md` + commit | `Lee _fundacion/ARRANQUE.md y ejecuta la Fase 1.` |
| 2 | `docs/blueprint/PRD.md` + commit | `Lee _fundacion/ARRANQUE.md y ejecuta la Fase 2.` |
| 3 | `docs/blueprint/Architecture.md` + commit | `Lee _fundacion/ARRANQUE.md y ejecuta la Fase 3.` |
| 4 | `memsys3/` desplegado + ADRs + commit | `Lee _fundacion/ARRANQUE.md y ejecuta la Fase 4.` |
| 5 | `README.md`, `AGENTS.md`, `CLAUDE.md` + commit | `Lee _fundacion/ARRANQUE.md y ejecuta la Fase 5.` |
| 6 | Limpieza, verificación, cierre de memoria + commit final | `Lee _fundacion/ARRANQUE.md y ejecuta la Fase 6.` |

**Una fase por sesión.** No las encadenes: cada fase necesita que la anterior
esté cerrada, y una sesión sobrecargada degrada justo los documentos finales. El
commit de cierre es lo que permite que la fase siguiente arranque sin depender
de la sesión anterior.

## Antes de empezar

Estás leyendo este archivo desde `_fundacion/`, así que el kit ya está en su
sitio. Falta dejar el repositorio listo para trabajar por fases:

```bash
git init          # solo si el repositorio todavía no está inicializado

cat >> .gitignore <<'EOF'
# Andamiaje de fundación (caedrus-kit) — se retira al cerrar la fundación
/_fundacion/
/deploy-config.yaml
/memsys3_temp/
EOF
```

Dos reglas sobre ese bloque:

- **Se añade al final (`>>`); nunca se sobrescribe.** Si el repositorio ya traía
  un `.gitignore`, sus reglas se conservan intactas.
- **Las tres rutas van ancladas con `/`.** Así quedan atadas al raíz del
  repositorio y restringidas a directorio, y no pueden ignorar por accidente
  algo del proyecto con un nombre parecido. Además ninguna línea empieza por
  `memsys3`: el despliegue de la Fase 4 lee ese prefijo para saber si la memoria
  quedó excluida de git, y una forma sin anclar le haría dar un aviso falso.

El bloque existe porque desde la Fase 1 hay un commit por fase, y el andamiaje
—el kit, la configuración del despliegue y el clon temporal— no debe entrar en
el historial del proyecto. La Fase 6 retira exactamente esas cuatro líneas y
nada más.

Ten a mano lo que sepas del proyecto (notas, un brief, ideas sueltas). No hace
falta que esté ordenado: la Fase 1 sirve precisamente para ordenarlo.

## Estado final esperado

Al terminar la Fase 6, la raíz del proyecto contiene **siempre** esto:

```
mi-proyecto/
├── .git/
├── docs/
│   └── blueprint/
│       ├── VisionScope.md
│       ├── PRD.md
│       └── Architecture.md
├── memsys3/
├── README.md
├── AGENTS.md
└── CLAUDE.md      # puente: solo importa AGENTS.md (ver Fase 5.3)
```

Puede contener además, **solo si** se cumple su condición:

- `.gitignore` — si el proyecto tenía reglas propias o eligió excluir `memsys3/`
  de git;
- `MEMORY.md` — solo si `memory_bridge: true` **y** comprobaste que tu harness
  lee ese archivo en esa ruta;
- los archivos propios del stack, si el repositorio ya tenía código.

Y no debe quedar nada de esto: `_fundacion/`, `deploy-config.yaml`,
`memsys3_temp/` ni el bloque de andamiaje dentro de `.gitignore`. El kit es
andamiaje: se retira cuando el edificio se sostiene solo.

---

# Instrucciones para el agente

Todo lo que sigue va dirigido al agente que ejecuta las fases.

## Contrato de interacción

Aplica en **todas** las fases, sin excepción:

1. **Pregunta antes de escribir.** No crees ni modifiques ningún archivo sin
   confirmación explícita del usuario. Propón, muestra, espera el visto bueno.
2. **Usa preguntas con opciones.** Si tu herramienta soporta preguntas
   estructuradas (`AskUserQuestion`, `questions` o equivalente), úsala siempre.
   Si no, numera las opciones en texto y marca cuál recomiendas y por qué.
3. **Pocas preguntas por ronda.** Máximo 3-4, y solo sobre vacíos críticos. Lo
   que puedas deducir de la fuente de verdad, dedúcelo y enséñalo para validar.
4. **No inventes.** Si un dato no sale del material del usuario ni de sus
   respuestas, pregúntalo. Un documento fundacional con relleno inventado es
   peor que uno incompleto.
5. **Idioma:** responde en el idioma en que te escribe el usuario y genera los
   documentos en ese mismo idioma, salvo que él indique otro.
6. **Cierra cada fase** con: qué se creó, qué decisiones se tomaron, el commit
   de la fase y la frase exacta que el usuario debe pegar para abrir la
   siguiente.

## Commit al cierre de cada fase

Las seis fases terminan con un commit, y ese commit hace dos trabajos:

- **Punto de retorno.** Si una fase sale mal, se vuelve a la anterior sin perder
  nada. La única fase destructiva es la 6, y es la última: cuando llega, todo el
  trabajo previo ya está versionado.
- **Continuidad entre sesiones.** `memsys3/` no existe hasta la Fase 4, así que
  hasta entonces el historial de git es la única memoria de la fundación.

De ahí la regla que gobierna el cuerpo del commit:

> Todo lo que la fase dejó claro y **no cabe** en su documento canónico
> —pendientes, alternativas descartadas y por qué se descartaron, dudas que
> quedaron abiertas, cosas que el usuario dijo de pasada— va al **cuerpo del
> commit** de esa fase. Es lo único que la sesión siguiente puede leer sin ti.

Antes de commitear, comprueba con `git status --short` que no aparecen
`_fundacion/`, `deploy-config.yaml` ni `memsys3_temp/`; si aparecen, falta el
bloque de andamiaje en `.gitignore` (ver *Antes de empezar*). Commits en el
idioma del proyecto, atómicos y sin firmas de agente: el asunto nombra la fase y
lo que produjo.

## Fase 1 — VisionScope (el porqué)

1. Lee `_fundacion/README.md` (índice del kit) y
   `_fundacion/templates/VisionScope.template.md` completo, incluida su cabecera
   de instrucciones.
2. **Busca antes de pedir.** Empieza por lo que ya hay en el repositorio:

   ```bash
   ls -A
   ```

   Si encuentras material previo —un `README.md`, un directorio de documentos,
   notas sueltas, un brief—, léelo entero **antes** de preguntar nada y enséñale
   al usuario la lista de lo que has usado. Solo cuando hayas comprobado que no
   hay material aprovechable se lo pides a él; y aun entonces, pregunta
   únicamente por lo que el material no resuelve.
3. Entrevístalo siguiendo las guías de la plantilla, sección por sección,
   respetando el Contrato de interacción. Preguntas que casi siempre hacen falta
   y conviene ofrecer con opciones:
   - **Idioma de trabajo** del proyecto (documentación y memoria).
   - **Fase actual**: Planificación / MVP / Beta / Producción.
   - **Límites y no-objetivos**: son los que más cuesta sacar y los que más
     drift evitan. Insiste con ejemplos concretos.
4. Cuando tengas material suficiente, **muestra el documento propuesto** y pide
   confirmación.
5. Al aprobarlo, crea `docs/blueprint/VisionScope.md`:
   - conserva solo el cuerpo desde `# VisionScope — ...`;
   - descarta la cabecera de instrucciones y el pie de la plantilla;
   - borra todos los comentarios de guía de la plantilla;
   - no dejes ningún `{{slot}}` sin rellenar: si una sección no aplica, bórrala
     o escribe `No aplica — <razón>`.
6. Verifica que no queda ningún slot y enséñale la salida al usuario:
   ```bash
   grep -nE '\{\{[^}]*\}\}' docs/blueprint/VisionScope.md \
     && echo "⚠️  quedan slots sin rellenar" || echo "✅ sin slots"
   ```

**Cierre:** resume las decisiones de alcance y los límites que quedaron fijados,
y **commitea la fase**. Al cuerpo del commit va lo que no cabe en el VisionScope:
lo que el usuario descartó y por qué, y las dudas que siguen abiertas.
Siguiente frase: `Lee _fundacion/ARRANQUE.md y ejecuta la Fase 2.`

## Fase 2 — PRD (el qué)

1. Lee `docs/blueprint/VisionScope.md` y `_fundacion/templates/PRD.template.md`.
2. Deriva del VisionScope todo lo que puedas (actores, necesidades, prioridades)
   y **preséntalo ya redactado** para que el usuario lo corrija. No le hagas
   repetir lo que ya dijo en la Fase 1.
3. Pregunta solo por lo que el VisionScope no resuelve: requisitos concretos,
   criterios de aceptación, qué queda fuera de esta iteración.
4. Numera `RF-01`, `RF-02`, `RNF-01`... de forma estable: esos identificadores
   se referencian después desde Architecture y desde los ADRs.
5. **Cada requisito debe ser verificable.** Si no se puede comprobar,
   reescríbelo con el usuario hasta que lo sea.
6. Muestra, confirma y crea `docs/blueprint/PRD.md` con las mismas reglas de
   limpieza de la Fase 1. Verifica que no queda ningún slot:
   ```bash
   grep -nE '\{\{[^}]*\}\}' docs/blueprint/PRD.md \
     && echo "⚠️  quedan slots sin rellenar" || echo "✅ sin slots"
   ```
7. Contrasta contra el VisionScope: si algo lo contradice, manda el VisionScope
   y el PRD se corrige. Avisa al usuario si detectas la contradicción.

**Cierre:** lista los RF/RNF numerados y **commitea la fase**. Al cuerpo del
commit van los pendientes que no pertenecen al PRD: lo que se aplazó a otra
iteración y lo que quedó sin decidir.
Siguiente frase: `Lee _fundacion/ARRANQUE.md y ejecuta la Fase 3.`

## Fase 3 — Architecture (el cómo)

1. Lee `docs/blueprint/VisionScope.md`, `docs/blueprint/PRD.md` y
   `_fundacion/templates/Architecture.template.md`.
2. Pregunta por el stack y las decisiones estructurales con opciones concretas y
   una recomendación razonada. Si el usuario no tiene criterio técnico formado,
   tu trabajo es acotarle el espacio de decisión, no abrírselo.
3. Describe la arquitectura **objetivo** si aún no hay código, y márcala como
   tal. No describas como existente lo que todavía no está.
4. Vincula los atributos de calidad a los `RNF-NN` del PRD.
5. Presta atención especial a la sección **Decisiones base (ADR-stubs)**: cada
   línea es la semilla de un ADR que se registrará en la Fase 4. Formato:
   `decisión — porque justificación`. Que no queden vacías.
6. Muestra, confirma y crea `docs/blueprint/Architecture.md` con las reglas de
   limpieza habituales. Verifica que no queda ningún slot:
   ```bash
   grep -nE '\{\{[^}]*\}\}' docs/blueprint/Architecture.md \
     && echo "⚠️  quedan slots sin rellenar" || echo "✅ sin slots"
   ```

**Cierre:** enumera las decisiones base que pasarán a ADR y **commitea la
fase**. Al cuerpo del commit van las alternativas que se descartaron y su
motivo: de ahí sale el contexto de los ADRs que se registran en la Fase 4.3.
Siguiente frase: `Lee _fundacion/ARRANQUE.md y ejecuta la Fase 4.`

## Fase 4 — memsys3 (memoria y continuidad)

### 4.1 — Puente blueprint → memsys3

1. Lee `_fundacion/templates/deploy-config.template.yaml`: cada campo indica de
   qué sección del blueprint se deriva.
2. Rellénalo leyendo `docs/blueprint/VisionScope.md` y `docs/blueprint/PRD.md`.
   No preguntes lo que ya está escrito ahí.
3. Quedan dos decisiones que **no** salen del blueprint. Pregúntalas con
   opciones, por el nombre de la clave y con su recomendación:
   - `gitignore_memsys3` — **¿excluir `memsys3/` de git?** Recomendado `false`,
     es decir, **no** excluirla: versionar la memoria del proyecto es lo que la
     protege contra la pérdida de contexto, y la deja disponible para cualquiera
     que clone el repositorio. Ponlo a `true` solo si el usuario tiene un motivo
     explícito para que la memoria no viaje con el código.
   - `memory_bridge` — ¿crear `MEMORY.md` en la raíz como puntero? Ponlo a
     `true` **solo si el usuario comprueba que su harness carga un `MEMORY.md`
     situado en la raíz del proyecto**. El disparador es la ruta que el harness
     lee, no su marca: Claude Code, por ejemplo, **no** la lee, porque su
     auto-memory vive en `~/.claude/projects/<hash>/memory/`. Si no está
     comprobado, `false`: un `MEMORY.md` que nadie carga es un archivo inerte en
     la raíz del proyecto.
4. Muestra el archivo y, tras confirmación, escríbelo en la **raíz** como
   `deploy-config.yaml`. No lo añadas al historial: el bloque de andamiaje del
   `.gitignore` ya lo mantiene fuera, y la Fase 6 lo borra.

### 4.2 — Deploy

1. Clona el sistema y sitúalo en la versión contra la que se probó este kit:
   ```bash
   git clone https://github.com/iv0nis/memsys3 memsys3_temp
   git -C memsys3_temp checkout b95eda9
   ```
   El `checkout` no es opcional: sin él tomas la rama por defecto, y lo que
   instalas deja de ser lo que `COMPATIBILITY.md` declara probado. Si ese commit
   ya no existe o prefieres una versión más reciente, sáltalo a sabiendas de que
   las ocho filas de dependencia de `COMPATIBILITY.md` dejan de estar respaldadas,
   y díselo al usuario.
2. Lee y ejecuta `memsys3_temp/memsys3_templates/prompts/deploy.md` paso a paso.
3. En su Paso 3 detectará `deploy-config.yaml` y entrará en **modo
   declarativo**: no repitas el briefing al usuario.
4. Su Paso 10 se comporta de dos maneras según lo que encuentre en la raíz.
   **Ninguna de las dos es un fallo del despliegue**, así que no lo des por roto
   ni intentes corregirlo aquí:
   - **Si el repositorio no tenía `AGENTS.md`**, creará uno genérico. Es
     esperado: se sustituye en la Fase 5.2. No lo edites ahora.
   - **Si el repositorio ya tenía `AGENTS.md`**, no lo tocará y avisará de que
     hay que mergear a mano el invariante de memoria. Ese aviso puedes
     ignorarlo: la plantilla de la Fase 5.2 ya trae esa sección, y es allí donde
     se resuelve. Dile al usuario que lo has visto y que está previsto.
5. El despliegue termina compilando `memsys3/memory/context.yaml`. Verifica que
   el archivo **no** quedó vacío:
   ```bash
   test -s memsys3/memory/context.yaml && echo "contexto OK"
   ```
   Si está vacío, revisa que `project-status.yaml` se escribió bien antes de
   continuar.

### 4.3 — ADRs de las decisiones base

1. Lee la sección *Decisiones base* de `docs/blueprint/Architecture.md`.
2. Lee `memsys3/prompts/adr.md` y sigue su procedimiento.
3. Propón un ADR por decisión, con contexto, alternativas descartadas y
   consecuencias. El cuerpo de los commits de las Fases 1-3 es tu fuente para
   esas alternativas. Muéstralos y espera confirmación antes de escribir en
   `memsys3/memory/full/adr.yaml`.
4. Respeta el orden y el formato que ya tenga el archivo, y actualiza su índice.

**Cierre:** confirma que existen `memsys3/`, `context.yaml` compilado y los ADRs
registrados, y **commitea la fase**. Al cuerpo del commit van los dos valores que
elegiste en 4.1 —`gitignore_memsys3` y `memory_bridge`— con su motivo: el
archivo que los contiene se borra en la Fase 6 y ningún ADR los recoge.
Siguiente frase: `Lee _fundacion/ARRANQUE.md y ejecuta la Fase 5.`

## Fase 5 — README y AGENTS (la capa operativa)

### 5.1 — README.md

1. Lee los tres documentos de `docs/blueprint/` y
   `_fundacion/templates/README.template.md` completo, incluida su cabecera de
   instrucciones.
2. Genera `README.md` describiendo **solo lo que existe hoy y es verificable**.
   Lo aspiracional ya vive en el blueprint; repetirlo aquí es lo que produce
   drift entre documentos.
3. La fase que declares debe coincidir con la de
   `memsys3/memory/project-status.yaml`: es el mismo dato en dos sitios, y si
   divergen, el README miente sobre el estado del proyecto.
4. En el mapa del repositorio lista **únicamente directorios que existan ya**.
   El andamiaje no va: `_fundacion/`, `deploy-config.yaml` y `memsys3_temp/`
   desaparecen en la Fase 6, y un mapa que los nombre nace caducado.
5. Muestra, confirma y escribe `README.md` con las reglas de limpieza
   habituales: solo el cuerpo, sin la cabecera de instrucciones ni el pie de la
   plantilla, sin comentarios de guía y sin ningún `{{slot}}` sin rellenar.
6. Verifica que no queda ningún slot y enséñale la salida al usuario:
   ```bash
   grep -nE '\{\{[^}]*\}\}' README.md \
     && echo "⚠️  quedan slots sin rellenar" || echo "✅ sin slots"
   ```

### 5.2 — AGENTS.md

1. Lee `_fundacion/templates/AGENTS.template.md`, el `README.md` que acabas de
   escribir y el `AGENTS.md` que haya hoy en la raíz.
2. Genera el `AGENTS.md` definitivo desde la plantilla, alineado con el
   blueprint y el README. Cómo lo escribes depende de lo que encontraste, y es
   la continuación del Paso 10 del despliegue (Fase 4.2):
   - **Si el `AGENTS.md` de la raíz es el genérico que creó el despliegue**,
     sustitúyelo entero: no conserva nada que valga la pena rescatar.
   - **Si el repositorio ya traía un `AGENTS.md` propio**, no lo tires. Sus
     reglas son del proyecto; tú aportas la estructura y el invariante de
     memoria. Propón el documento fusionado, señala qué sección tuya desplaza a
     cuál suya y espera confirmación sección por sección.
3. **Obligatorio:** conserva literal la sección
   `## Invariante de memoria agnóstica (ADR-027)`, con ese título exacto y su
   cuerpo. El título es interfaz, no estilo: la comprobación de más abajo lo
   busca tal cual, y reescribirlo *"para que quede mejor"* rompe algo que
   nadie ve romperse.
4. Borra las secciones opcionales que no apliquen —*Estándares de código* si aún
   no hay código, *Skills y herramientas* si el usuario no usa ninguna—. Mejor
   corto y cierto que largo y decorativo.
5. Muestra, confirma, escribe y verifica antes de cerrar:
   ```bash
   grep -c "Invariante de memoria agnóstica (ADR-027)" AGENTS.md   # debe dar 1
   grep -nE '\{\{[^}]*\}\}' README.md AGENTS.md \
     && echo "⚠️  quedan slots sin rellenar" || echo "✅ sin slots"
   ```

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

**Cierre:** confirma que existen `README.md`, `AGENTS.md` y `CLAUDE.md`, y
**commitea la fase**. Al cuerpo del commit van las secciones de la plantilla que
borraste y por qué, y —si el repositorio ya traía un `AGENTS.md` propio— qué
reglas suyas conservaste y cuáles quedaron desplazadas: el repaso de coherencia
de la Fase 6 arranca justo de ahí.
Siguiente frase: `Lee _fundacion/ARRANQUE.md y ejecuta la Fase 6.`

## Fase 6 — Limpieza, verificación y cierre de memoria

1. Muestra al usuario **qué vas a borrar** y espera **confirmación explícita**
   antes de ejecutar nada. Es la única fase destructiva del arranque:
   - `_fundacion/` — el kit; ya cumplió su función.
   - `deploy-config.yaml` — consumido por el despliegue en la Fase 4.
   - `memsys3_temp/` — el clon del sistema de memoria, si el despliegue no lo
     eliminó ya.
   - el bloque de andamiaje del `.gitignore` — el comentario y sus tres rutas.

   Antes de borrar nada, **anota la versión del kit**: la declara
   `_fundacion/CHANGELOG.md`. El paso 5 la registra en la memoria del proyecto,
   y es el único dato del andamiaje que tiene que sobrevivir a este borrado.

   > Avísale de que las plantillas seguirán disponibles en el kit original por
   > si algún día quiere regenerar un documento desde cero.

   Si no confirma, no borras nada y la fase se detiene aquí. No hay borrado
   parcial ni *"voy empezando por lo evidente"*.
2. Tras la confirmación, borra el andamiaje:
   ```bash
   rm -rf _fundacion deploy-config.yaml memsys3_temp
   ```

   Y retira el bloque del `.gitignore`. Esta parte **la haces tú, abriendo el
   archivo**, y no con un comando: la edición en sitio no se comporta igual en
   todos los sistemas y aquí se opera sobre un archivo que puede ser del
   proyecto. La regla es exacta:
   - borra la línea `# Andamiaje de fundación (caedrus-kit) — se retira al
     cerrar la fundación` y las **tres** que la siguen —`/_fundacion/`,
     `/deploy-config.yaml` y `/memsys3_temp/`—, y **nada más**;
   - todo lo demás se conserva intacto: las reglas propias del proyecto y, si la
     Fase 4 lo escribió, el bloque de exclusión de `memsys3/`;
   - si al retirar el bloque el archivo **queda vacío**, bórralo: era solo
     andamiaje. Si queda cualquier otra línea, el `.gitignore` es del proyecto y
     se queda donde está.
3. Verifica el estado final y **enséñale la salida** al usuario:
   ```bash
   ls -A   # contrastar con el inventario condicional
   grep -nE '\{\{[^}]*\}\}' docs/blueprint/*.md README.md AGENTS.md \
     && echo "⚠️  quedan slots sin rellenar" || echo "✅ sin slots"
   grep -c "Invariante de memoria agnóstica (ADR-027)" AGENTS.md   # debe dar 1
   grep -c "^@AGENTS.md" CLAUDE.md   # debe dar 1
   test -s memsys3/memory/context.yaml && echo "contexto OK"
   ```

   La cuarta línea es, carácter a carácter, la misma que la de la Fase 5.3, y
   así debe quedarse: dos expresiones distintas para el mismo invariante son
   peores que la imprecisión de una.

   La salida de `ls -A` no se contrasta contra una lista fija, sino contra este
   inventario de tres categorías. Cada entrada cae en una sola:

   **Siempre.** Si falta alguna, algo salió mal antes: `.git/`, `docs/`,
   `memsys3/`, `README.md`, `AGENTS.md`, `CLAUDE.md`.

   **Solo si.** Correcto únicamente cuando se cumple su condición; si no se
   cumple, se retira antes de cerrar:
   - `.gitignore` — si el proyecto tenía reglas propias o eligió excluir
     `memsys3/` de git. Si el bloque de andamiaje era todo su contenido, el
     paso 2 ya lo borró.
   - `MEMORY.md` — solo si `memory_bridge: true` **y** comprobaste que tu
     harness lee ese archivo en esa ruta. Si no lo comprobaste, es un archivo
     inerte en la raíz del proyecto: bórralo ahora.
   - los archivos propios del stack, si el repositorio ya tenía código.

   **Residuo.** No debe quedar nada de esto: `_fundacion/`,
   `deploy-config.yaml`, `memsys3_temp/` y el bloque de andamiaje dentro de
   `.gitignore`. Los tres primeros los enseña `ls -A`; el cuarto se comprueba
   abriendo el archivo. Si aparece alguno, el paso 2 quedó a medias.
4. Repasa la coherencia entre documentos: precedencia
   `VisionScope > PRD > Architecture > README > AGENTS`. Si detectas una
   contradicción, dila explícitamente en vez de arreglarla por tu cuenta: el
   usuario decide qué documento cede.
5. **Cierre de memoria del proyecto fundado.** Hasta aquí, la memoria describe
   el despliegue de la Fase 4: `estado_actual.ultima_feature` sigue diciendo
   *"Deployment inicial de memsys3"* y no hay ninguna sesión registrada. Si
   cierras sin este paso, la primera sesión real del proyecto abrirá una memoria
   que describe un estado que ya no existe.

   **Material.** Los seis commits de fase son la fuente:
   ```bash
   git log --oneline
   ```
   Léelos enteros, también los cuerpos: ahí está lo que no cupo en los
   documentos canónicos.

   **Mecanismo.** Lee y ejecuta `memsys3/prompts/endSession.md`, y déjale a él
   el formato: el kit no reproduce el schema de `sessions.yaml` ni el de
   `project-status.yaml`. Si ese prompt no está en tu proyecto, cumple el mismo
   contrato de contenido leyendo los schemas que el despliegue instaló en
   `memsys3/memory/templates/sessions-template.yaml` y
   `memsys3/memory/templates/project-status-template.yaml`.

   **Contrato de contenido.** Lo pone el kit, porque el prompt documenta *la
   sesión* y la sesión en curso es solo la Fase 6:
   - la entrada de `sessions.yaml` describe **la fundación completa, de la
     Fase 1 a la 6**, no la limpieza que acabas de hacer;
   - y cita la **versión del kit** con la que se fundó, la que anotaste en el
     paso 1: es el único sitio donde ese dato sobrevive al andamiaje. Va en
     `sessions.yaml`, que es texto libre, y **no** como clave nueva de
     `project-status.yaml`;
   - `estado_actual.ultima_feature` deja de decir *"Deployment inicial de
     memsys3"* y pasa a describir la fundación;
   - `siguiente_milestone` **se lo preguntas al usuario**, con opciones. No lo
     deduzcas del blueprint: es una decisión suya, no una derivación;
   - `pendientes_prioritarios` recoge lo que quedó abierto de verdad —las dudas
     de los cuerpos de commit, lo que destapó el paso 4—, y no la fundación,
     que ya está cerrada.

   Dos cosas que **no** se hacen aquí:
   - **No recompiles `memsys3/memory/context.yaml`.** El despliegue ya lo
     compiló y esta sesión viene cargada. Limítate a informar al usuario de
     cuántas sesiones quedan sin compilar y de que le conviene recompilar al
     abrir la siguiente.
   - Si la atribución que propone el prompt lleva el nombre de un harness que no
     es el tuyo, **pon el tuyo**. Sustituyes el valor que escribes; no editas el
     prompt.
6. **Commitea la fundación.** Es el commit final y va después del cierre de
   memoria, para que la memoria ya actualizada entre en él. Pregunta con
   opciones si el usuario quiere además un **tag inicial** —`v0.1.0` es la
   elección habitual— y ponlo solo si lo confirma. Al cuerpo del commit va lo
   que este cierre dejó decidido y no vive en ningún documento: el milestone que
   el usuario eligió y qué se resolvió en cada contradicción del paso 4.
7. Explica cómo se trabaja a partir de ahora:
   - abrir sesión: `memsys3/prompts/newSession.md`
   - cerrar sesión: `memsys3/prompts/endSession.md`
   - recompilar el contexto: `memsys3/prompts/compile-context.md`
   - registrar decisiones: `memsys3/prompts/adr.md`
   - si cambia la dirección del proyecto: **primero** `docs/blueprint/`, después
     `README.md` y `AGENTS.md`, y luego se registra en memsys3. Ese orden es la
     precedencia del paso 4 aplicada al día a día.

   El despliegue instala más prompts en `memsys3/prompts/`; estos cuatro son los
   del trabajo diario, no la lista completa.

**Cierre:** el proyecto está fundado. El andamiaje ya no está, la memoria
describe el estado real y el historial tiene un commit por fase. A partir de
aquí, desarrollo normal: no hay Fase 7 ni frase que pegar.
