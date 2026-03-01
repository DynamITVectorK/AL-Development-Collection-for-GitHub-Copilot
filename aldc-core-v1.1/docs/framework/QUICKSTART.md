# Quickstart ALDC Core v1.1

## Objetivo

En 30 minutos, pasar de "repo con Copilot" a "repo con ALDC Core v1.1" funcionando.

## Requisitos previos

- VS Code con extensión GitHub Copilot
- Repo AL (Business Central) con `app.json`
- Node.js 20+ (para el validador)

## Setup rápido (15 min)

### 1. Estructura base

Asegura que existen estos directorios y ficheros:

```
aldc.yaml                              ← configuración Core
.github/copilot-instructions.md        ← entrypoint Copilot
.github/plans/memory.md                ← memory global (desde template)

agents/                                ← 4 agentes públicos
  al-architect.agent.md
  al-conductor.agent.md
  al-developer.agent.md
  al-presales.agent.md
  orchestra/                           ← 2 subagents internos
    al-planning-subagent.agent.md
    al-review-subagent.agent.md

prompts/                               ← 6 workflows
  al-spec.create.prompt.md
  al-build.prompt.md
  al-pr-prepare.prompt.md
  al-memory.create.prompt.md
  al-context.create.prompt.md
  al-initialize.prompt.md

skills/                                ← 11 skills composables
  skill-api.md
  skill-copilot.md
  skill-debug.md
  skill-performance.md
  skill-events.md
  skill-permissions.md
  skill-testing.md
  skill-migrate.md
  skill-pages.md
  skill-translate.md
  skill-estimation.md
  index.md

instructions/                          ← 9 instructions (auto-applied)
docs/templates/                        ← 7 plantillas inmutables
tools/aldc-validate/                   ← validador
```

### 2. Inicializar memory global

Copia `docs/templates/memory-template.md` → `.github/plans/memory.md`. Rellena Project Info.

### 3. Validar

```bash
cd tools/aldc-validate && npm install js-yaml
cd ../..
node tools/aldc-validate/index.js --config aldc.yaml
```

Resultado esperado: `✅ ALDC Core v1.1 COMPLIANT`

## Flujo por requerimiento (15 min de práctica)

### Requerimiento LOW (sin arquitectura)

1. Asignar nombre: `{req_name}` en kebab-case (ej: `customer-discount`)
2. `@workspace use al-spec.create` → genera `.github/plans/customer-discount.spec.md`
3. `@al-developer` → implementa directamente (carga skills según necesidad)
4. Actualizar `memory.md` con resultado

### Requerimiento MEDIUM/HIGH (con arquitectura + TDD)

1. Asignar nombre: `{req_name}` (ej: `api-integration`)
2. `@workspace use al-spec.create` → genera `.github/plans/api-integration.spec.md`
3. `@al-architect` → genera `.github/plans/api-integration.architecture.md`
   - ⚠️ **GATE**: aprobar arquitectura antes de continuar
4. Crear `.github/plans/api-integration.test-plan.md` desde template
5. Actualizar `memory.md` con contexto del nuevo requerimiento
6. `@al-conductor` → orquesta ciclo TDD:
   - Planning (subagent) → Implementation (al-developer) → Review (subagent)
   - ⚠️ **GATE**: validación humana por fase
7. `@workspace use al-pr-prepare` → documentación PR
8. Actualizar `memory.md` con resultado
9. Archivar set completado en `.github/plans/archive/` (opcional)

### Ejemplo de `.github/plans/` tras un requerimiento

```
.github/plans/
  memory.md                              ← GLOBAL (proyecto)
  api-integration.spec.md               ← per-requirement
  api-integration.architecture.md
  api-integration.test-plan.md
```

## Routing de agentes

| ¿Qué necesitas? | Agente |
|---|---|
| Diseñar arquitectura, analizar, decidir | `@al-architect` |
| Implementar, codificar, debuggear | `@al-developer` |
| Feature TDD completa (plan → impl → review → commit) | `@al-conductor` |
| Estimar proyecto, sizing, propuesta | `@al-presales` |

Los agentes cargan skills automáticamente según el contexto de la tarea. No necesitas invocar skills directamente.

## Checklist de onboarding

- [ ] `aldc.yaml` presente y válido
- [ ] `.github/copilot-instructions.md` presente
- [ ] `.github/plans/memory.md` creado desde plantilla
- [ ] `docs/templates/` con 7 plantillas inmutables
- [ ] 4 agentes + 2 subagents presentes
- [ ] 6 workflows presentes
- [ ] 11 skills presentes en `skills/`
- [ ] 9 instructions presentes
- [ ] `aldc-validate` pasa ✅
- [ ] Ejemplo LOW ejecutado con `{req_name}` y spec
- [ ] Ejemplo MEDIUM ejecutado con conductor, subagentes y requirement set completo
