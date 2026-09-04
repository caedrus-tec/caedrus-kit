<!-- version: 0.4.0 -->
<!--
============================================================
TEMPLATE: docs/blueprint/PRD.md  (capa canónica · doc 2/3)
============================================================
Propósito: definir el QUÉ debe ofrecer el proyecto y con qué
criterios se considera correcto/útil. Deriva de VisionScope.

Cómo rellenar:
  1. Parte de VisionScope.md ya cerrado (no lo contradigas).
  2. Para cada sección responde la pregunta de su comentario Guía;
     lo no resuelto por la fuente de verdad, pregúntalo al usuario.
  3. Sustituye cada {{slot}} y numera RF/RNF de forma estable.
  4. Cada requisito debe ser verificable: si no se puede comprobar,
     reescríbelo hasta que lo sea.
  5. Al generar, conserva solo el cuerpo desde "# PRD — ...";
     descarta esta cabecera de instrucciones de la template.

QUÉ va aquí:  usuarios, necesidades, requisitos funcionales y no
              funcionales, métricas/KPIs, criterios de aceptación,
              out-of-scope, dependencias, prioridades, roadmap.
QUÉ NO va aquí:
  - el porqué, la visión y los límites estratégicos   -> VisionScope.md
  - cómo se implementa, módulos, stack, contratos      -> Architecture.md

Precedencia documental: VisionScope > PRD > Architecture > README > AGENTS
Agnóstico de modelo de IA.
============================================================
-->

# PRD — {{PROYECTO}}

**Version:** 0.1.0  
**Status:** Borrador  
**Date:** {{YYYY-MM-DD}}

## Propósito del documento

<!-- Guía: ¿qué define este PRD y para quién? Una frase de encuadre. -->
{{proposito}}

## Usuarios y actores

<!-- Guía: por cada actor de VisionScope, ¿qué necesita resolver con el proyecto? Un sub-bloque por actor. -->
### {{actor_1}}
{{necesidad_actor_1}}

### {{actor_2}}
{{necesidad_actor_2}}

## Necesidades principales

<!-- Guía: necesidades transversales que el producto debe satisfacer, independientes de un actor concreto. -->
- {{necesidad_1}}
- {{necesidad_2}}

## Requisitos funcionales

<!-- Guía: qué DEBE hacer el sistema. Numera RF-01, RF-02... Cada uno con nombre y condición verificable. Usa MUST/SHOULD/MAY si ayuda. -->
### RF-01 {{titulo_rf_1}}
{{descripcion_rf_1}}

### RF-02 {{titulo_rf_2}}
{{descripcion_rf_2}}

## Requisitos no funcionales

<!-- Guía: cómo de bien debe comportarse (rendimiento, trazabilidad, claridad, seguridad, portabilidad...). Numera RNF-01... -->
### RNF-01 {{titulo_rnf_1}}
{{descripcion_rnf_1}}

### RNF-02 {{titulo_rnf_2}}
{{descripcion_rnf_2}}

## Métricas / KPIs

<!-- Guía: indicadores medibles que reflejan que los requisitos se cumplen en la práctica. Vincula cada uno a un RF/RNF cuando sea posible. -->
- {{kpi_1}}
- {{kpi_2}}

## Criterios de aceptación

<!-- Guía: lista verificable de condiciones que, si se cumplen TODAS, el proyecto está "hecho" en esta iteración. Redacta en presente, comprobable. -->
1. {{criterio_1}}
2. {{criterio_2}}
3. {{criterio_3}}

## Fuera de alcance (out of scope)

<!-- Guía: lo que este PRD explícitamente NO cubre en esta iteración (aunque pueda venir después). Evita expectativas. -->
- {{fuera_de_alcance_1}}
- {{fuera_de_alcance_2}}

## Dependencias y supuestos

<!-- Guía: ¿de qué sistemas, datos, accesos o decisiones depende? ¿qué se asume cierto para que el PRD se sostenga? -->
- {{dependencia_o_supuesto_1}}
- {{dependencia_o_supuesto_2}}

## Prioridades

<!-- Guía: clasifica el trabajo actual por urgencia/valor. No metas todo en Alta. -->
### Alta
- {{prioridad_alta_1}}

### Media
- {{prioridad_media_1}}

### Baja
- {{prioridad_baja_1}}

## Roadmap inicial

<!-- Guía: secuencia de etapas para entregar el PRD. Cada etapa con un resultado claro. No es un cronograma con fechas. -->
### Etapa 1
{{etapa_1}}

### Etapa 2
{{etapa_2}}

### Etapa 3
{{etapa_3}}

---

<!-- Siguiente eslabón: con PRD cerrado, deriva Architecture.md (el cómo) desde
     _fundacion/templates/Architecture.template.md.
     Pipeline global: docs/blueprint -> memsys3 -> README -> AGENTS. -->
