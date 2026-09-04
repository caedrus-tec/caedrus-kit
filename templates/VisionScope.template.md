<!-- version: 0.4.0 -->
<!--
============================================================
TEMPLATE: docs/blueprint/VisionScope.md  (capa canónica · doc 1/3)
============================================================
Propósito: definir el PORQUÉ y el ALCANCE del proyecto. Es la
fuente de verdad superior; ante un conflicto, manda VisionScope.

Cómo rellenar:
  1. Lee la fuente de verdad disponible (repo, brief, notas).
  2. Para cada sección responde la pregunta de su comentario Guía.
     Lo que no salga de la fuente de verdad, pregúntalo al
     usuario (pocas preguntas por ronda, solo vacíos críticos).
  3. Sustituye cada {{slot}} por contenido real y borra las guías.
  4. No avances a PRD/Architecture hasta que VisionScope sea coherente.
  5. Al generar, conserva solo el cuerpo desde "# VisionScope — ...";
     descarta esta cabecera de instrucciones de la template.

QUÉ va aquí:  visión, problema/oportunidad, alcance, límites,
              no-objetivos, actores, objetivos, métricas, riesgos
              estratégicos, principios, fases.
QUÉ NO va aquí:
  - requisitos concretos y criterios de aceptación   -> PRD.md
  - módulos, stack, boundaries, contratos técnicos    -> Architecture.md

Precedencia documental: VisionScope > PRD > Architecture > README > AGENTS
Agnóstico de modelo de IA: no asumas un harness/modelo concreto.
============================================================
-->

# VisionScope — {{PROYECTO}}

**Version:** 0.1.0  
**Status:** Borrador  
**Date:** {{YYYY-MM-DD}}

## Vision

<!-- Guía: ¿qué es {{PROYECTO}} en 1-2 frases? ¿cuál es su propósito último, el cambio que busca producir? -->
{{vision}}

## Problema y oportunidad

<!-- Guía: ¿qué duele hoy / qué falla sin este proyecto? ¿por qué ahora? ¿qué oportunidad abre resolverlo? -->
{{problema_oportunidad}}

## Alcance

<!-- Guía: ¿qué cubre el proyecto? Lista lo que SÍ entra. Concreto y verificable. -->
- {{alcance_item_1}}
- {{alcance_item_2}}
- {{alcance_item_3}}

## Límites

<!-- Guía: ¿qué queda fuera por decisión de diseño (no por falta de tiempo)? ¿qué NO pretende ser? -->
- {{limite_1}}
- {{limite_2}}

## No-objetivos

<!-- Guía: metas que alguien podría asumir pero que explícitamente NO perseguimos (evita expectativas erróneas). -->
- {{no_objetivo_1}}
- {{no_objetivo_2}}

## Actores principales

<!-- Guía: ¿quién usa, opera o se beneficia del proyecto? Humanos y/o agentes. Una línea por actor con su necesidad. -->
- {{actor_1}} — {{necesidad_1}}
- {{actor_2}} — {{necesidad_2}}

## Objetivos estratégicos

<!-- Guía: 3-5 resultados de alto nivel que definen el éxito a medio plazo. -->
1. {{objetivo_1}}
2. {{objetivo_2}}
3. {{objetivo_3}}

## Métricas de éxito / señales

<!-- Guía: ¿cómo sabremos que vamos bien? Señales observables o métricas, aunque sean cualitativas. Evita métricas de vanidad. -->
- {{metrica_o_senal_1}}
- {{metrica_o_senal_2}}

## Supuestos y riesgos estratégicos

<!-- Guía: supuestos de los que depende la visión + riesgos que podrían invalidarla. Marca cada uno como [supuesto] o [riesgo]. -->
- {{supuesto_o_riesgo_1}}
- {{supuesto_o_riesgo_2}}

## Principios rectores

<!-- Guía: reglas no negociables que guían cualquier decisión. Ej.: "evidencia versionada sobre narrativa aspiracional". -->
- {{principio_1}}
- {{principio_2}}

## Estado y contexto actual

<!-- Guía: ¿en qué punto real está el proyecto hoy (idea, prototipo, en producción, refactor)? Describe el estado observable, no el aspiracional. -->
{{estado_actual}}

## Fases / hitos iniciales

<!-- Guía: divide el camino en fases con un objetivo claro cada una. No es un cronograma; es una secuencia lógica. -->
### Fase 1 — {{nombre_fase_1}}
- {{hito_1_1}}
- {{hito_1_2}}

### Fase 2 — {{nombre_fase_2}}
- {{hito_2_1}}

### Fase 3 — {{nombre_fase_3}}
- {{hito_3_1}}

---

<!-- Siguiente eslabón: con VisionScope cerrado, deriva PRD.md (el qué) desde
     _fundacion/templates/PRD.template.md y luego Architecture.md (el cómo) desde
     _fundacion/templates/Architecture.template.md.
     Pipeline global: docs/blueprint -> memsys3 -> README -> AGENTS. -->
