# CHANGELOG

Historia de versiones de este kit. Cada entrada dice **qué cambia para quien lo
usa**, no qué párrafos se retocaron.

El kit se versiona **como conjunto**: una sola versión en SemVer para todos sus
archivos. Las plantillas de `templates/` llevan además un marcador de versión
propio en su primera línea; ningún otro archivo del kit lo lleva.

## v1.0.0 — 2026-08-24

**Primera versión distribuida.** Hubo antes una **versión anterior, interna, no
distribuida**, que nunca se publicó ni se ofreció a nadie de fuera. Todo lo que
sigue se describe respecto a ella.

### Añadido

- **Bloque de `.gitignore` de andamiaje.** El quickstart lo escribe por append,
  antes del primer commit: un bloque marcado con tres rutas ancladas al raíz
  —`/_fundacion/`, `/deploy-config.yaml` y `/memsys3_temp/`—, que nunca
  sobrescribe un `.gitignore` existente. La Fase 6 retira exactamente ese bloque
  y nada más, y borra el archivo solo si queda vacío.
  *Qué cambia:* el andamiaje —el kit, la configuración del despliegue y el clon
  temporal del sistema de memoria— deja de entrar en el historial del proyecto
  fundado. La versión anterior no escribía `.gitignore` en ninguna fase; con un
  commit por fase, todo eso habría quedado versionado, y borrarlo al final no lo
  saca del historial.

- **Paso 5 nuevo en la Fase 6: cierre de la memoria del proyecto fundado.** Va
  después del repaso de coherencia y **antes** del commit final. Ejecuta el
  prompt de cierre de sesión del sistema de memoria y le impone un contrato de
  contenido: la entrada de sesión describe la fundación completa —de la Fase 1 a
  la 6, tomando como material los seis commits de fase—, cita la versión del kit
  con la que se fundó, y el estado del proyecto deja de decir que lo último que
  ocurrió fue el despliegue de la memoria. Es el **"tramo 6.5"** que pedía la
  corrección pendiente: en el runbook se llama **paso 5** y en ningún sitio
  *"6.5"*, porque a un lector nuevo ese número no le dice nada; los pasos 1 a 4
  no se renumeran.
  *Qué cambia:* antes, la fundación terminaba dejando la memoria del proyecto
  describiendo un estado que ya no existía, y la primera sesión real del proyecto
  abría pidiendo cerrar una fundación que ya estaba cerrada.

- **Commit al cierre de cada fase, ahora en las seis.** Al cuerpo del commit va
  además lo que no cabe en el documento canónico de esa fase: pendientes,
  alternativas descartadas y dudas abiertas.
  *Qué cambia:* seis puntos de retorno donde antes no había ninguno, y
  continuidad entre sesiones antes de que exista `memsys3/` —hasta la Fase 4, el
  historial de git es la única memoria de la fundación—. La única fase
  destructiva es la última, y cuando llega, todo el trabajo previo ya está
  versionado.

- **Cinco archivos nuevos**, ninguno existía en la versión anterior:
  `LITERALS.md` (manifiesto de literales congelados), `COMPATIBILITY.md`
  (dependencia externa, acoplamientos y defectos heredados), `LIMITS.md` (qué no
  está probado), `CHANGELOG.md` (este archivo) y `LICENSE` (MIT, `caedrus-tec`).

- **U4 entre los defectos heredados**, declarado desde `v1.0.0`. El prompt de
  cierre de sesión del sistema de memoria externo guarda un identificador de
  agente fuera del proyecto y propone la atribución de la sesión con un nombre de
  harness escrito a mano. La observación quedó **confirmada contra el commit
  probado**; el detalle, con su marca de origen, está en `COMPATIBILITY.md`.
  *Qué cambia:* el paso 5 de la Fase 6 instruye sustituir esa atribución cuando
  no coincide con el ejecutor. Lo que ese prompt escribe fuera del proyecto no lo
  cubre nada del kit.

### Cambiado

- **Las seis plantillas se mudan al subdirectorio `templates/`.** Dejan de estar
  en la raíz del kit. `ARRANQUE.md` se queda en la raíz.
  *Qué cambia:* la raíz del kit pasa a ser lo que se lee para decidir si usarlo,
  y `templates/` el material que las fases rellenan. Las rutas que el runbook
  nombra son ahora `_fundacion/templates/...`; la frase con la que se arranca
  cada fase no cambia, porque el runbook no se ha movido de sitio.

### Versionado

- **La versión pasa a ser de conjunto.** `v1.0.0` versiona a la vez los trece
  archivos del kit.
- **El bump pendiente del runbook queda absorbido.** La corrección pendiente
  pedía subir `ARRANQUE.md` de `0.2.0` a `0.3.0`. Al quedarse fuera de
  `templates/`, el runbook **pierde su marcador de versión por archivo** y pasa a
  versionarse con el conjunto: ese bump **queda absorbido por `v1.0.0`** y no es
  una tarea pendiente. Sin este registro, alguien lo dará por incumplido.
- **Marcador de versión solo en `templates/`**, en la línea 1:
  `<!-- version: X.Y.Z -->` en Markdown y `# version: X.Y.Z` en YAML.

Versiones de las plantillas en `v1.0.0`:

| Plantilla | Versión |
|---|---|
| `VisionScope.template.md` | 0.4.0 |
| `PRD.template.md` | 0.4.0 |
| `Architecture.template.md` | 0.4.0 |
| `README.template.md` | 0.3.0 |
| `AGENTS.template.md` | 0.4.0 |
| `deploy-config.template.yaml` | 0.3.0 |

### Corregido antes de publicar

Una lectura completa de los trece archivos, hecha por alguien sin contexto previo
sobre el kit, encontró cinco cosas que se arreglaron antes de esta primera
publicación. Se registran porque describen en qué se equivocaba el kit, no solo qué
cambió:

- **Las reservas no llegaban a quien ejecuta.** `ARRANQUE.md` y las seis plantillas
  —los siete archivos que se leen durante la fundación— no citaban `LIMITS.md`,
  `COMPATIBILITY.md` ni `LITERALS.md` en ninguna línea. Quien seguía el runbook sin
  abrir la raíz del kit no encontraba ni una sola reserva. El runbook las nombra ahora
  en su encabezado.
- **La marca `verificado@` prometía más de lo que cubría.** Significa que se leyó el
  texto del sistema externo en un commit congelado, y dos filas la usaban para
  afirmar comportamiento en ejecución (*"los prompts entran"*, *"existe y escribe"*).
  El desmentido estaba escrito en `LIMITS.md`, en el archivo de al lado. La definición
  de la marca lo dice ahora, hay una tercera regla de uso, y las dos filas están
  reescritas.
- **Cuatro rutas afirmadas sin respaldo:** `memory/memory.yaml`, `memory/context.yaml`,
  `memory/full/adr.yaml` y `memory/templates/memory-template.yaml`. La primera es el
  destino del invariante congelado `A2`: el kit congelaba como interfaz un texto que
  apunta a un archivo cuya existencia no había comprobado. Las cuatro se contrastaron
  contra el commit probado y entran como **octava fila** de la tabla de dependencias.
- **El `checkout` que fija la versión no estaba donde se clona.** `COMPATIBILITY.md`
  explicaba que hacía falta; `ARRANQUE.md` clonaba la rama por defecto y no lo
  mencionaba. Ahora lo ejecuta la propia Fase 4.2.
- **Los pies de las cinco plantillas Markdown** citaban las rutas sin el prefijo
  `_fundacion/` que usa el runbook, y nombraban una herramienta que ninguna fase
  instala ni explica. Corregido el prefijo y retirada la mención.

Una sexta observación de esa lectura **no se corrigió, se declaró**: el runbook afirma,
dentro de un literal congelado, que el harness descarta los comentarios HTML antes de
inyectar el archivo del puente en contexto. Es conducta interna de un programa, que
este kit no puede verificar, y el texto se conserva porque reescribirlo rompería la
propiedad que hace comprobable la cadena de literales. Queda registrada como
desviación conocida en `LITERALS.md` y precisada en `COMPATIBILITY.md`. Nada de lo que
el runbook manda hacer depende de que sea cierta.

Un repaso posterior, ya sobre el kit corregido, encontró dos cosas más:

- **La sección que informa del estado de la dependencia externa no declaraba el
  suyo.** `LIMITS.md` pide que cada una de sus secciones diga en qué punto está lo
  que describe, y era la única de las seis que no lo hacía, además de la única que
  abre en positivo. Aislado de lo que viene después, su primer párrafo podía leerse
  como una garantía. Lleva ahora uno de los tres estados que el propio archivo
  define: `probado en un caso`, y solo sobre texto.
- **Una afirmación sobre el comportamiento de un harness, dentro de otro literal
  congelado.** El aviso de puente describe cómo un harness concreto carga dos
  archivos de instrucciones, y `LIMITS.md` declara que solo uno ha ejecutado el
  runbook entero. La afirmación no es infundada —es lo que sostiene usar un import
  y no un enlace simbólico—, pero lo que la respalda no vive en este kit. El texto
  se conserva por la misma razón que la observación anterior; lo que se hizo fue
  precisar `D7`, que define ahora qué cuenta como *probado* y extiende esa reserva
  a todas las afirmaciones del kit sobre harness. Segunda desviación conocida de
  ese literal en `LITERALS.md`.

Los marcadores de versión de `templates/` no se tocan: nadie ha visto todavía una
versión anterior de esas plantillas, así que estos arreglos forman parte de `v1.0.0` y
no de un incremento sobre ella.

### Dependencia externa

Contra qué versión del sistema de memoria se contrastó `v1.0.0` está en
`COMPATIBILITY.md`, fila por fila y con su marca de origen. Cuando esa
dependencia avance, el cambio de versión probada se anota aquí.
