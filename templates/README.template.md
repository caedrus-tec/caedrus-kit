<!-- version: 0.3.0 -->
<!--
============================================================
TEMPLATE: README.md  (capa operativa · derivado del blueprint)
============================================================
Propósito: aterrizar la base canónica al ESTADO OBSERVABLE del
repositorio. El README no redefine el proyecto: lo traduce a lo
que hoy existe y a cómo se empieza a trabajar en él.

Cuándo se rellena: con docs/blueprint/ cerrado Y memsys3 ya
desplegado. Antes de eso no puedes describir memoria ni comandos
reales; los estarías inventando.

Cómo rellenar:
  1. Lee docs/blueprint/VisionScope.md, PRD.md y Architecture.md.
  2. Describe SOLO lo verificable en el repo hoy. Lo aspiracional
     pertenece al blueprint, no aquí.
  3. Responde la pregunta del comentario Guía de cada sección y
     borra las guías.
  4. Regla dura para el mapa y para los comandos: no enumeres una
     ruta que no exista ya en la raíz, ni un comando que no hayas
     ejecutado en este repositorio. Un README que ofrece algo que
     no está es el defecto más caro de esta capa: nadie lo detecta
     hasta que alguien lo prueba y falla.
  5. Sustituye cada {{slot}} por contenido real y borra los que no
     uses. Si una sección entera no aplica, bórrala.
  6. Al generar, conserva solo el cuerpo desde "# PROYECTO";
     descarta esta cabecera de instrucciones y el pie.

QUÉ va aquí:  identidad breve, estado observable, mapa del repo,
              modelo de memoria, arranque, convenciones, comandos.
QUÉ NO va aquí:
  - visión, alcance y límites        -> docs/blueprint/VisionScope.md
  - requisitos y criterios           -> docs/blueprint/PRD.md
  - módulos, boundaries, contratos   -> docs/blueprint/Architecture.md
  - reglas de operación de agentes   -> AGENTS.md

Precedencia documental: VisionScope > PRD > Architecture > README > AGENTS
Agnóstico de modelo de IA: no asumas un harness concreto.
============================================================
-->

# {{PROYECTO}}

**Version:** {{vX.Y.Z}}

<!-- Guía: ¿qué es el proyecto en 1-2 frases? Toma la Vision del VisionScope y redúcela a su forma más concreta y observable. -->
{{descripcion_breve}}

## Propósito del repo

<!-- Guía: ¿para qué existe este repositorio? 3-5 bullets derivados del Alcance del VisionScope, en términos de lo que el repo contiene y sostiene. -->
Este repositorio existe para:

- {{proposito_1}}
- {{proposito_2}}
- {{proposito_3}}

## Estado actual

<!-- Guía: estado real, no aspiracional. La fase debe coincidir con la de memsys3/memory/project-status.yaml. -->
- **Fase:** {{fase}}
- **Situación actual:** {{situacion_actual}}
- **Dirección vigente:** {{direccion_vigente}}
- **Pendiente principal:** {{pendiente_principal}}

## Fuentes de verdad y precedencia documental

Si existe conflicto entre documentos, prevalece este orden:

`VisionScope > PRD > Architecture > README > AGENTS`

Fuentes y rol:

1. `docs/blueprint/VisionScope.md` — visión, problema, alcance y límites.
2. `docs/blueprint/PRD.md` — necesidades, requisitos y criterios de aceptación.
3. `docs/blueprint/Architecture.md` — módulos, boundaries, contratos y flujos base.
4. `README.md` — aterriza la base canónica al estado observable del repo.
5. `AGENTS.md` — operacionaliza el trabajo de agentes sobre esa base.
6. `memsys3/` — memoria auditable, backlog, sesiones y contexto operativo.

`CLAUDE.md` no entra en esta cadena: es un puente que importa `AGENTS.md`, no una fuente propia.

## Modelo de memoria

`memsys3/` gestiona la continuidad operativa del proyecto:

- `memsys3/memory/context.yaml` es el contexto compilado para trabajar.
- `memsys3/memory/project-status.yaml` expresa el estado operativo actual.
- `memsys3/memory/memory.yaml` conserva perfil del usuario y feedback aprendido.
- `memsys3/memory/full/adr.yaml` y `memsys3/memory/full/sessions.yaml` conservan historia detallada.
- `memsys3/backlog/` registra trabajo futuro.

Punto clave: `memsys3/` NO reemplaza a `docs/blueprint/`. Si la historia y la definición actual entran en conflicto, manda `docs/blueprint/`.

## Mapa del repositorio

<!-- Guía: una línea por directorio o archivo de primer nivel que exista HOY. Deriva de las
     Capas y módulos de Architecture.md, pero lista solo lo que ya está creado.
     Las cinco primeras entradas existen siempre al terminar la fundación: consérvalas.
     Añade a continuación, y solo si ya están en la raíz de este repositorio:
       - `.gitignore`, si el proyecto tiene reglas propias o excluye `memsys3/` de git;
       - `MEMORY.md`, solo si tu harness lee de verdad ese archivo en esa ruta;
       - los directorios y archivos propios del stack, si el repositorio ya tiene código.
     Usa los slots de abajo para esas entradas y borra los que no rellenes. -->
- `docs/blueprint/` — base canónica del proyecto: `VisionScope.md`, `PRD.md` y `Architecture.md`.
- `memsys3/` — memoria, contexto, prompts y backlog.
- `README.md` — este documento: estado observable del repositorio.
- `AGENTS.md` — reglas de operación para agentes.
- `CLAUDE.md` — puente de harness: importa `AGENTS.md` para los agentes que no lo leen directamente.
- {{ruta_propia_1}} — {{descripcion_propia_1}}
- {{ruta_propia_2}} — {{descripcion_propia_2}}

## Estado observable por área

<!-- Guía: por cada área del mapa que merezca matiz, en qué punto real está y qué papel juega hoy. Un sub-bloque por área. Omite las triviales. -->
### `docs/blueprint/`

Fija la intención del proyecto. Debe actualizarse antes que cualquier documento operativo cuando cambia la dirección.

### `memsys3/`

Núcleo operativo de memoria y gobernanza. Es la fuente de continuidad real entre sesiones.

### {{area_3}}

{{estado_area_3}}

## Flujo recomendado para empezar

1. Leer `docs/blueprint/` para entender intención, alcance y arquitectura base.
2. Leer este `README.md` para aterrizar el estado observable del repo.
3. Leer `AGENTS.md` para reglas operativas de agentes.
4. Cargar contexto desde `memsys3/memory/context.yaml`.
5. Usar los prompts de `memsys3/` para sesión, memoria y continuidad.

## Convenciones generales

<!-- Guía: reglas prácticas del día a día (idioma, formato, qué no se toca a mano). Deriva de los Principios del blueprint, pero en forma accionable. -->
- Documentación y comunicación en {{idioma}}.
- Código y términos técnicos en inglés.
- No editar `memsys3/memory/context.yaml` manualmente.
- {{convencion_4}}
- Tras cambios estructurales, revisar si `README.md` y `AGENTS.md` siguen alineados con `docs/blueprint/`.

## Comandos útiles

<!-- Guía: los cuatro prompts de memoria de abajo están en este repositorio y puedes dejarlos tal
     cual. Es a propósito un subconjunto conservador: `memsys3/prompts/` contiene más, y este
     README solo enumera lo que se sostiene en cualquier estado del proyecto. Antes de añadir
     nada al slot propio, ejecútalo aquí y comprueba que funciona; un comando que este repositorio
     no tiene no se documenta, se borra. -->
Prompts de memoria disponibles en este repositorio. La lista es un subconjunto conservador, no un
inventario exhaustivo: `memsys3/prompts/` contiene más.

- Iniciar sesión: `memsys3/prompts/newSession.md`
- Cerrar sesión: `memsys3/prompts/endSession.md`
- Compilar contexto: `memsys3/prompts/compile-context.md`
- Registrar decisión (ADR): `memsys3/prompts/adr.md`

Comandos propios del proyecto (build, test, run u operativa):

- {{comando_propio_1}} — {{para_que_sirve_1}}
- {{comando_propio_2}} — {{para_que_sirve_2}}

---

<!-- Siguiente eslabón: con README cerrado, deriva AGENTS.md (cómo opera un agente sobre
     esta base) desde _fundacion/templates/AGENTS.template.md.
     Pipeline global: docs/blueprint -> memsys3 -> README -> AGENTS. -->
