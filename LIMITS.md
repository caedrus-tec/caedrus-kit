# LIMITS.md — qué no está probado

Este archivo no matiza lo que el kit hace: enumera lo que nadie ha comprobado que
haga. Existe como archivo propio, y no como una sección al final del `README.md`,
porque un límite sin domicilio termina absorbido por el tono del documento que lo
aloja.

Regla de escritura, que vale también para quien lo edite mañana: **ninguna entrada
se cierra con una tranquilidad.** Si una frase de aquí cabría en el `README.md` sin
chirriar, ha dejado de describir un límite y hay que reescribirla.

`v1.0.0` se publica con **una sola validación de campo**, ejecutada los días 19 y 20
de agosto de 2026. Todo lo que sigue se deriva de ese hecho.

## Cómo leer las entradas

| Estado | Significado |
|---|---|
| `no probado` | no existe ninguna ejecución registrada que respalde la afirmación |
| `probado en un caso` | hay una ejecución, y solo una: no separa lo que hace el kit de las condiciones en que se ejecutó |
| `pendiente` | requisito que el kit declara y no cumple, con la prueba que falta enumerada |

---

## B1 — El Contrato de interacción no es evaluable en la máquina donde se validó

**Estado:** `no probado`.

El kit le pide a su ejecutor dos cosas: no escribir ningún archivo sin aprobación
explícita del usuario, y preguntar con opciones en vez de en abierto. En la
validación de campo el agente hizo las dos.

Eso no prueba nada sobre el kit. La configuración global del usuario que ejecutó esa
validación ya obligaba a ambas cosas a cualquier agente, dijera lo que dijese el
documento que estuviera leyendo. Un kit con el contrato roto —o directamente sin
contrato— habría producido la misma observación. La medición no distingue entre las
dos causas, así que no mide.

**Qué falta para poder afirmar algo:** ejecutar el runbook con un ejecutor cuya
configuración no imponga ninguna de las dos reglas, y comprobar si el comportamiento
se sostiene con lo que dice el runbook y nada más. Hasta entonces el Contrato de
interacción es una declaración del kit, no un comportamiento observado.

## B2 — El puente `CLAUDE.md` → `AGENTS.md` no tiene prueba limpia

**Estado:** `no probado`.

La Fase 5.3 crea `CLAUDE.md` con un import de `AGENTS.md` porque hay harness que no
leen `AGENTS.md`. La prueba de que el puente funciona sería que el agente use
contenido de `AGENTS.md` sin haber podido obtenerlo por ninguna otra vía.

En la única validación no fue así. Varios archivos del repositorio fundado
mencionaban el invariante de memoria, y el agente había leído algunos de ellos en la
misma fase. Que lo citara no prueba que llegara por el puente: prueba que estaba
disponible por más de un camino y que no se registró cuál se usó. La contaminación es
de construcción, no un descuido del ejecutor: el kit escribe ese invariante en varios
sitios a propósito, de modo que el contenido más fácil de comprobar es justamente el
peor candidato a servir de sonda.

**Qué falta para poder afirmar algo:** una sesión nueva, sin lecturas previas, en un
repositorio donde el contenido bajo prueba exista **solo** dentro de `AGENTS.md`, y
una pregunta cuya respuesta correcta no pueda salir de ningún otro archivo. Con el
resultado registrado, esa misma sonda cierra también parte de D7.

## B3 — Una sola muestra, y muy particular

**Estado:** `probado en un caso`.

Lo que se ejecutó, con todas sus condiciones a la vista:

| Dimensión | Valor único de la muestra |
|---|---|
| Dominio | un proyecto de aprendizaje |
| Harness | Claude Code |
| Idioma | castellano |
| Material de partida | abundante |
| Estado del repositorio | vacío |
| Ejecutor | un agente en frío, una fase por sesión |

Con `n = 1` no hay forma de separar lo que aporta el kit de lo que aportaban esas
seis condiciones. Y estos casos **no se han ejecutado nunca**:

- **Repositorio que ya tiene código.** Nunca ejecutado, y es el caso que un producto
  público recibe el primer día. El runbook lo nombra en su inventario de estado
  final; nombrarlo no es haberlo ejecutado, y ninguna de esas menciones se apoya en
  una ejecución.
- **Otro harness.** Ver D7, más abajo.
- **Otro idioma.** El kit está escrito en castellano y solo se ha ejecutado en
  castellano. Un ejecutor que trabaje en otro idioma sobre un runbook en castellano
  es un caso sin observar.
- **Proyecto sin material de partida.** La Fase 1 busca material en el repositorio y,
  si no encuentra nada, se lo pide al usuario. La rama *"no encuentra nada"* no se ha
  recorrido.
- **Persona sin agente.** Las fases están redactadas como instrucciones a un ejecutor
  agente. Nadie ha seguido el runbook a mano de principio a fin.
- **Repositorio con un flujo de ramas propio.** El runbook **propone** un commit al
  cierre de cada fase; no impone rama, ni política de revisión, ni flujo de PR, y no
  se ha ejecutado donde exista alguno de los tres.

## B4 — El falso positivo de la comprobación de slots era condicional, no estructural

**Estado:** `no probado`.

La comprobación de slots sin rellenar aparece en seis puntos del runbook. La versión
anterior buscaba la secuencia de apertura suelta, y por eso podía marcar como slot
pendiente una simple mención a las plantillas dentro de un documento generado. Ese
fallo **no se disparaba en todas las fases**: dependía de que el archivo revisado
contuviera una apertura sin cerrar, condición que solo se daba en algunas.

De ahí salen dos cosas que el kit no puede decir. No puede decir que corrige un fallo
observado en seis sitios: lo observado fue un fallo posible en seis sitios y visto
donde se cumplió la condición. Y a la inversa, que en la validación las demás fases
no dieran falsos positivos no prueba que la expresión anterior fuera correcta allí;
prueba que la condición no se dio.

Sobre la expresión actual tampoco hay medición: solo encuentra slots que abren y
cierran en la misma línea, porque la búsqueda es por líneas, y ningún documento de la
validación tenía un slot partido en dos.

## D7 — Agnosticismo de harness: **pendiente**

**Estado:** `pendiente`. Ligado a B2.

El kit declara soportar los agentes que leen `AGENTS.md` —OpenCode, Claude Code,
Codex y cualquier otro con esa convención—, y **solo Claude Code está probado**. En
Claude Code lo que se probó fue la fundación completa; el puente que ese harness
necesita sigue sin prueba limpia (B2). D7 no está cumplido, y ninguna frase del kit
puede leerse como agnosticismo verificado.

*Probado* aquí significa una cosa concreta: haber ejecutado las seis fases de este
runbook en ese harness. Ninguna afirmación de este kit sobre cómo un harness carga
archivos equivale a haberlo ejecutado en él, y ninguna se apoya en una ejecución
registrada aquí.

Qué prueba faltaría, en términos ejecutables por quien lea esto:

1. Ejecutar las **seis fases enteras**, una por sesión, sobre un repositorio vacío,
   en OpenCode y en Codex, con el mismo material de partida en ambos.
2. Registrar por fase: si el agente localizó el runbook con la frase de entrada y
   nada más; si las comprobaciones del cierre corrieron sin adaptar ningún comando;
   y si la Fase 4 completó el despliegue del sistema de memoria externo.
3. En la Fase 5.3, comprobar en un harness que lee `AGENTS.md` de forma nativa que el
   import no le carga el documento dos veces: `test -f CLAUDE.md` y
   `grep -c "^@AGENTS.md" CLAUDE.md`, y luego una pregunta cuya respuesta revele si
   el contenido llegó duplicado.
4. Cerrar B2 con su sonda en cada harness, no solo en uno.

Hasta que existan esas ejecuciones registradas, lo que el kit puede decir de sí mismo
es que está escrito para no depender de un harness, no que se haya comprobado que no
depende.

## Estado de la verificación de la dependencia externa

**Estado:** `probado en un caso`, y solo sobre texto.

La verificación contra el sistema de memoria externo **sí se ejecutó**: el 21 de
agosto de 2026 se clonó `iv0nis/memsys3` en el commit `b95eda9` y se contrastaron
contra ese checkout las afirmaciones dependientes de `COMPATIBILITY.md`; el 24 se
recontrastaron contra ese mismo checkout, sin cambios. Ninguna fila de ese archivo
quedó sin clon.

El alcance de ese hecho es exactamente el que dice: se leyó **qué contiene** el
upstream en un commit congelado. No se ejecutó su despliegue desde ese checkout, así
que lo verificado es el texto de la dependencia, no su comportamiento al correr, ni
el de ninguna versión posterior.
