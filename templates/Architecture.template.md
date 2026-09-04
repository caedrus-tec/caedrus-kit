<!-- version: 0.4.0 -->
<!--
============================================================
TEMPLATE: docs/blueprint/Architecture.md  (capa canónica · doc 3/3)
============================================================
Propósito: definir el CÓMO — estructura, módulos, boundaries y
contratos que sostienen el QUÉ del PRD. Deriva de PRD y VisionScope.

Cómo rellenar:
  1. Parte de VisionScope y PRD ya cerrados; no los contradigas.
  2. Responde el comentario Guía de cada sección desde la fuente de
     verdad (estado real del repo) + interacción con el usuario.
  3. Describe la arquitectura observable/objetivo, no una ideal sin
     evidencia. Marca lo aspiracional como tal.
  4. Sustituye cada {{slot}} y borra las guías.
  5. Al generar, conserva solo el cuerpo desde "# Architecture — ...";
     descarta esta cabecera de instrucciones de la template.

QUÉ va aquí:  principios técnicos, stack, capas/módulos, boundaries,
              contratos, modelo de datos, atributos de calidad,
              flujos, riesgos técnicos, decisiones base.
QUÉ NO va aquí:
  - el porqué y el alcance             -> VisionScope.md
  - el qué y criterios de aceptación   -> PRD.md

Precedencia documental: VisionScope > PRD > Architecture > README > AGENTS
Agnóstico de modelo de IA.
============================================================
-->

# Architecture — {{PROYECTO}}

**Version:** 0.1.0  
**Status:** Borrador  
**Date:** {{YYYY-MM-DD}}

## Propósito arquitectónico

<!-- Guía: ¿qué problema estructural resuelve esta arquitectura? ¿qué busca evitar (mezcla de responsabilidades, drift...)? -->
{{proposito_arquitectonico}}

## Principios de arquitectura

<!-- Guía: reglas técnicas que guían cualquier decisión de diseño. Ej.: "separación explícita de responsabilidades", "evolución sin sobreescritura ciega". -->
- {{principio_arq_1}}
- {{principio_arq_2}}

## Stack tecnológico

<!-- Guía: lenguajes, frameworks, formatos, herramientas y servicios. Si el repo es doc-first, descríbelo igual. Marca lo confirmado vs lo tentativo. -->
- {{stack_item_1}}
- {{stack_item_2}}

## Capas y módulos principales

<!-- Guía: por cada capa/módulo: responsabilidad, artefactos observables y restricciones. Un sub-bloque por módulo. -->
### {{modulo_1}}
**Responsabilidad:** {{responsabilidad_1}}  
**Artefactos:** {{artefactos_1}}  
**Restricción:** {{restriccion_1}}

### {{modulo_2}}
**Responsabilidad:** {{responsabilidad_2}}  
**Artefactos:** {{artefactos_2}}  
**Restricción:** {{restriccion_2}}

## Boundaries

<!-- Guía: fronteras explícitas entre módulos/preocupaciones. Por cada boundary, qué separa y cómo se resuelve un conflicto. -->
### Boundary 1 — {{boundary_1}}
{{descripcion_boundary_1}}

### Boundary 2 — {{boundary_2}}
{{descripcion_boundary_2}}

## Contratos técnicos y documentales

<!-- Guía: acuerdos que no se rompen. La precedencia documental ya está abajo; añade los contratos propios del proyecto (memoria, API, datos...). -->
### Contrato de precedencia
Toda contradicción se resuelve con este orden:
`VisionScope > PRD > Architecture > README > AGENTS`

### {{contrato_2}}
{{descripcion_contrato_2}}

## Modelo de datos / estado

<!-- Guía: ¿qué entidades/estado maneja el sistema y dónde viven? Esquemas, archivos canónicos, almacenes. Si no aplica, indícalo. -->
{{modelo_datos}}

## Atributos de calidad

<!-- Guía: cómo la arquitectura sostiene los RNF del PRD (portabilidad, trazabilidad, rendimiento, seguridad...). Vincula a RNF-NN. -->
- {{atributo_calidad_1}}
- {{atributo_calidad_2}}

## Flujos principales

<!-- Guía: secuencias de trabajo clave, paso a paso. Un sub-bloque por flujo. Incluye fundación/operación si aplica. -->
### Flujo 1 — {{flujo_1}}
1. {{paso_1_1}}
2. {{paso_1_2}}

### Flujo 2 — {{flujo_2}}
1. {{paso_2_1}}

## Riesgos técnicos y mitigaciones

<!-- Guía: por cada riesgo técnico relevante, su impacto y cómo se mitiga o se detecta. -->
- {{riesgo_1}} → {{mitigacion_1}}
- {{riesgo_2}} → {{mitigacion_2}}

## Decisiones base (ADR-stubs)

<!-- Guía: decisiones arquitectónicas ya tomadas, en forma breve. Cada una es semilla de una futura ADR (en memsys3). Formato: decisión — porque justificación. -->
- {{decision_1}} — porque {{justificacion_1}}
- {{decision_2}} — porque {{justificacion_2}}

---

<!-- Cierre del blueprint. Con los 3 docs coherentes, sigue el pipeline:
     docs/blueprint -> memsys3 (registrar decisiones base como ADRs) -> README
     (desde _fundacion/templates/README.template.md) -> AGENTS
     (desde _fundacion/templates/AGENTS.template.md). -->
