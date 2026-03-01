# CLAUDE.md — ALDC Core v1.1 Skills Simplification

## Project Context

This is the ALDC (AL Development Collection for GitHub Copilot) repository. We are migrating from v2.11.0 (38 primitives: 11 agents + 18 prompts + 9 instructions) to a skills-based architecture (ALDC Core v1.1).

The normative spec is at `docs/framework/ALDC-Core-Spec-v1.1.md`. Read it first.

## Current Task

Generate the real content for the v1.1 simplification. This involves:

### A. Create 11 skill files in `skills/`

Extract content from the agents/prompts being consolidated. Each skill MUST follow `docs/templates/skill-template.md`.

| Skill to create | Extract content from |
|-----------------|---------------------|
| `skills/skill-api.md` | `agents/al-api.agent.md` + `prompts/al-api.prompt.md` (if exists) |
| `skills/skill-copilot.md` | `agents/al-copilot.agent.md` + `prompts/al-copilot-capability.prompt.md` + `prompts/al-copilot-promptdialog.prompt.md` + `prompts/al-copilot-generate.prompt.md` + `prompts/al-copilot-test.prompt.md` |
| `skills/skill-debug.md` | `agents/al-debugger.agent.md` + `prompts/al-diagnose.prompt.md` |
| `skills/skill-performance.md` | `prompts/al-performance.prompt.md` + `prompts/al-performance.triage.prompt.md` + `instructions/al-performance.instructions.md` (active parts only) |
| `skills/skill-events.md` | `prompts/al-events.prompt.md` + `instructions/al-events.instructions.md` (active parts only) |
| `skills/skill-permissions.md` | `prompts/al-permissions.prompt.md` |
| `skills/skill-testing.md` | `agents/al-tester.agent.md` + `instructions/al-testing.instructions.md` (active parts only) |
| `skills/skill-migrate.md` | `prompts/al-migrate.prompt.md` |
| `skills/skill-pages.md` | `prompts/al-pages.prompt.md` |
| `skills/skill-translate.md` | `prompts/al-translate.prompt.md` |
| `skills/skill-estimation.md` | `agents/al-presales.agent.md` (estimation-specific content) |

**Rules for skill creation:**
- Read the source file(s) first
- Extract the domain knowledge, patterns, workflows, AL code examples
- Reorganize into skill-template structure (Purpose, When to Load, Core Patterns, Workflow, References, Constraints)
- Keep under 500 lines per skill
- Do NOT duplicate passive rules that already exist in instructions (instructions are auto-applied, skills are on-demand)
- Skills MUST NOT have agent frontmatter

### B. Update 4 public agents

Update these agents to reference skills conditionally:

1. `agents/al-architect.agent.md` — Add skill loading for api, copilot, performance, events
2. `agents/al-conductor.agent.md` — Update frontmatter: `agents: ['al-planning-subagent', 'al-review-subagent', 'al-developer']`, `tools: ['runSubagent', ...]`
3. `agents/al-developer.agent.md` — Add skill loading for debug, api, copilot, events, permissions, pages, migrate, translate
4. `agents/al-presales.agent.md` — Add skill loading for estimation

Skill loading pattern to use in agent files:
```markdown
## Domain Skills

When the task involves API design or implementation, load and follow:
- @file skills/skill-api.md

When debugging is needed, load and follow:
- @file skills/skill-debug.md
```

### C. Reorganize subagents

1. Create `agents/orchestra/` directory
2. Move `agents/al-planning-subagent.agent.md` → `agents/orchestra/al-planning-subagent.agent.md`
3. Move `agents/al-review-subagent.agent.md` → `agents/orchestra/al-review-subagent.agent.md`
4. Ensure both have `user-invokable: false` in frontmatter
5. Delete `agents/al-implement-subagent.agent.md` (absorbed by al-developer)

### D. Archive consolidated agents/prompts

Move to `archive/v2.11.0/` (create directory):
- `agents/al-debugger.agent.md`
- `agents/al-tester.agent.md`
- `agents/al-api.agent.md`
- `agents/al-copilot.agent.md`
- `agents/al-implement-subagent.agent.md`
- All 12 prompts being absorbed into skills (see Changelog)

### E. Update copilot-instructions.md

Simplify the routing to 4 agents:
- Designing/architecture? → `@al-architect`
- Implementing/coding/debugging? → `@al-developer`
- TDD orchestrated feature? → `@al-conductor`
- Project estimation? → `@al-presales`

### F. Update prompts (6 retained workflows)

Verify these 6 workflows reference the correct agents/skills:
- `prompts/al-spec.create.prompt.md`
- `prompts/al-build.prompt.md`
- `prompts/al-pr-prepare.prompt.md`
- `prompts/al-memory.create.prompt.md`
- `prompts/al-context.create.prompt.md`
- `prompts/al-initialize.prompt.md`

## Execution Order

1. Read the spec: `docs/framework/ALDC-Core-Spec-v1.1.md`
2. Read the skill template: `docs/templates/skill-template.md`
3. Create skills (A) — one by one, reading source files first
4. Update agents (B)
5. Reorganize subagents (C)
6. Archive old files (D)
7. Update copilot-instructions (E)
8. Verify workflows (F)
9. Run validator: `node tools/aldc-validate/index.js --config aldc.yaml`

## Important Constraints

- Do NOT modify instruction files (9 files, unchanged)
- Do NOT modify templates in `docs/templates/` (immutable)
- Do NOT delete `memory.md` or overwrite its content
- Keep all skills under 500 lines
- Use `@file` references for skill loading in agents (GitHub Copilot convention)
- Subagents in `agents/orchestra/` MUST have `user-invokable: false`
