# ALDC Manifesto

ALDC existe para convertir el desarrollo AL con agentes en ingeniería controlada.

En la era de la IA, el riesgo no es "que el agente no sepa programar".
El riesgo es **programar sin contrato**, sin trazabilidad y sin governance.

ALDC Core v1.1 impone:

- **Spec-first** — requisitos normalizados antes de tocar código (`{req_name}.spec.md`)
- **Architecture-first** — diseño aprobado antes de implementación en MEDIUM/HIGH (`{req_name}.architecture.md`)
- **Orquestación multi-agente con TDD** — conductor coordina planning, implementation y review via subagents
- **Gates HITL** — el humano decide en cada punto crítico (complejidad, arquitectura, fase, review, entrega)
- **Memory global** — continuidad de contexto entre sesiones y requerimientos (`memory.md`, append-only)
- **Contratos por requerimiento** — 3-file sets por cada req, no genéricos (`{req_name}.{type}.md`)
- **Plantillas inmutables** — 7 templates que solo el maintainer modifica vía RFC
- **Skills composables** — conocimiento especializado que agentes cargan bajo demanda, no agentes monolíticos
- **Extensión-only** — sin romper upgrades, sin modificar objetos base
- **Event-driven y API-first** — integración por eventos y APIs como patrón por defecto

## Modelo simplificado

4 agentes públicos. 1 pregunta para elegir:

- ¿Diseñando? → `al-architect`
- ¿Implementando? → `al-developer`
- ¿Feature TDD completa? → `al-conductor`
- ¿Estimando proyecto? → `al-presales`

Los agentes cargan skills especializados (api, copilot, debug, performance, events, permissions, testing, migrate, pages, translate, estimation) según lo que necesiten. Menos agentes, más conocimiento disponible.

## Objetivo

Menos "vibe coding"; más entrega predecible, auditable y segura.
