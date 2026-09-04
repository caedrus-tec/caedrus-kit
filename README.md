# caedrus-kit

Un kit de fundación para proyectos nuevos: lleva un repositorio vacío hasta un
proyecto con base canónica, memoria operativa y documentos alineados, listo para
empezar a desarrollar.

No es una librería ni un binario. Son trece archivos de texto que tu agente lee
y ejecuta contigo, en **6 fases y una sesión por fase**. Cuando el proyecto se
sostiene solo, el kit se retira: el andamiaje no sobrevive a la fundación.

## Qué produce

Al cerrar la Fase 6, el repositorio tiene:

- **Base canónica en `docs/blueprint/`** — `VisionScope.md` (el porqué),
  `PRD.md` (el qué) y `Architecture.md` (el cómo), escritos con el usuario
  sección por sección, a partir del material que ya tenga.
- **Memoria operativa en `memsys3/`** — el kit conduce el despliegue del sistema
  de memoria externo del que depende (ver `COMPATIBILITY.md`), registra como
  ADRs las decisiones estructurales de la arquitectura y cierra la fundación en
  la memoria del proyecto antes del commit final.
- **`README.md` y `AGENTS.md`** del proyecto, derivados de esa base, y el puente
  **`CLAUDE.md`**, que se limita a importar `AGENTS.md` para los agentes que no
  leen ese archivo.
- **Un historial con un commit por fase**: seis puntos de retorno, y lo que no
  cabe en un documento canónico —pendientes, alternativas descartadas, dudas
  abiertas— escrito en el cuerpo de esos commits.

Cada fase se abre pegándole una frase a tu agente y se cierra con un commit. Los
pasos, los comandos y las verificaciones viven en `ARRANQUE.md`; este archivo no
los repite.

## Qué hay dentro

Trece archivos: siete en la raíz y seis plantillas.

**En la raíz**

- **`README.md`** — este archivo: qué produce el kit, cómo se instala y qué deja
  al terminar.
- **`ARRANQUE.md`** — el runbook: las 6 fases con sus pasos, sus preguntas y sus
  verificaciones. Es el documento que el agente ejecuta de principio a fin.
- **`LITERALS.md`** — manifiesto de literales congelados: los fragmentos que son
  interfaz y no estilo, con su texto verbatim y cómo comprobar que siguen
  intactos.
- **`COMPATIBILITY.md`** — de qué sistema externo depende la Fase 4, contra qué
  versión se contrastó cada afirmación, los acoplamientos entre las dos piezas y
  los defectos heredados que llegan al proyecto fundado.
- **`LIMITS.md`** — qué no está probado, enunciado como límite y no como matiz.
- **`CHANGELOG.md`** — historia de versiones del kit. Es donde consta la versión
  con la que estás fundando.
- **`LICENSE`** — MIT, `caedrus-tec`.

**En `templates/`**

- **`VisionScope.template.md`** — el porqué del proyecto: propósito, alcance,
  límites y no-objetivos (Fase 1).
- **`PRD.template.md`** — el qué: requisitos numerados y verificables (Fase 2).
- **`Architecture.template.md`** — el cómo, con las decisiones base que después
  se registran como ADRs (Fase 3).
- **`README.template.md`** — el `README.md` del proyecto fundado (Fase 5.1).
- **`AGENTS.template.md`** — el `AGENTS.md` del proyecto fundado, incluida la
  sección del invariante de memoria (Fase 5.2).
- **`deploy-config.template.yaml`** — el puente declarativo entre el blueprint y
  el despliegue de la memoria (Fase 4.1).

## Quickstart

Desde la raíz del repositorio donde quieres fundar el proyecto:

```bash
git clone --depth 1 https://github.com/caedrus-tec/caedrus-kit _fundacion && rm -rf _fundacion/.git
```

El `rm -rf _fundacion/.git` no es opcional: sin él te queda un `.git/` anidado
dentro de tu proyecto, con el historial del kit metido en el tuyo.

Después, deja el repositorio listo para trabajar por fases:

```bash
git init          # solo si el repositorio todavía no está inicializado

cat >> .gitignore <<'EOF'
# Andamiaje de fundación (caedrus-kit) — se retira al cerrar la fundación
/_fundacion/
/deploy-config.yaml
/memsys3_temp/
EOF
```

Ese bloque se añade al final (`>>`) y nunca sobrescribe un `.gitignore` que ya
existiera: mantiene el andamiaje fuera del historial del proyecto, y la Fase 6 lo
retira. El porqué de cada línea está en `ARRANQUE.md`, sección *Antes de
empezar*.

Ya está. Abre una sesión con tu agente y pégale esta frase:

```
Lee _fundacion/ARRANQUE.md y ejecuta la Fase 1.
```

Cada fase termina diciéndote la frase con la que abrir la siguiente.

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

## Versión, autoría e idioma

- **Versión: `v1.0.0`.** El kit se versiona **como conjunto**: una sola versión
  para los trece archivos, declarada en `CHANGELOG.md`. Las plantillas llevan
  además un marcador propio en su primera línea, para saber cuál se usó al
  generar un documento; ningún otro archivo del kit lo lleva.
- **Dónde queda esa versión.** La Fase 6 la anota en la memoria del proyecto
  fundado: es el único sitio donde el dato sobrevive al borrado del andamiaje.
- **Autoría:** `caedrus-tec`.
- **Licencia:** MIT. Texto completo en `LICENSE`.
- **Idioma: castellano, por decisión explícita.** El kit está escrito en
  castellano; los términos técnicos, los nombres de archivo y el código van en
  inglés. Los documentos que produce son otra cosa: el runbook pide generarlos
  en el idioma en que hables con tu agente.

## Antes de decidir si te sirve

Dos archivos, y este `README.md` no resume ninguno de los dos:

- **`COMPATIBILITY.md`** — la Fase 4 despliega un sistema de memoria que el kit
  no trae dentro: es software de terceros, con su repositorio y su ritmo de
  cambios. Ahí está de qué versión suya depende lo que se probó, con qué marca
  de origen se sostiene cada afirmación, por dónde están soldadas las dos piezas
  y qué defectos suyos llegan al proyecto fundado.
- **`LIMITS.md`** — qué no está probado. `v1.0.0` se publica con **una sola
  validación de campo**, y de ahí se deriva todo lo que el kit no puede afirmar
  de sí mismo. El kit declara soportar los agentes que leen `AGENTS.md`
  —OpenCode, Claude Code, Codex y cualquier otro con esa convención—, y de todos
  ellos **solo Claude Code está probado**. Los límites están enunciados enteros
  ahí, y ahí es donde hay que leerlos.
