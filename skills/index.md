# ALDC Skills Catalog

## What are Skills?

Skills are composable knowledge modules in markdown format that agents load on demand based on task context. They encapsulate domain-specific patterns, workflows, and best practices.

Skills are NOT invocable directly — agents reference them via `@file` when the task requires specialized knowledge.

## Required Skills (MUST exist)

| Skill | Domain | Loaded by |
|-------|--------|-----------|
| [skill-api](skill-api.md) | API design, OData/REST, versioning | architect, developer |
| [skill-copilot](skill-copilot.md) | Copilot capability lifecycle | architect, developer |
| [skill-debug](skill-debug.md) | Debugging, profiling, root cause | developer |
| [skill-performance](skill-performance.md) | Performance patterns, triage | architect, developer |
| [skill-events](skill-events.md) | Event subscriber/publisher patterns | architect, developer |
| [skill-permissions](skill-permissions.md) | Permission sets, security | developer |
| [skill-testing](skill-testing.md) | Test strategy, Given/When/Then | conductor, developer |

## Recommended Skills (SHOULD exist)

| Skill | Domain | Loaded by |
|-------|--------|-----------|
| [skill-migrate](skill-migrate.md) | Version migration, breaking changes | developer |
| [skill-pages](skill-pages.md) | Page types, UX patterns | developer |
| [skill-translate](skill-translate.md) | XLF, multi-language | developer |
| [skill-estimation](skill-estimation.md) | Project estimation, SWOT | presales |

## Creating New Skills

1. Copy `docs/templates/skill-template.md` to `skills/skill-{domain}.md`
2. Fill in all sections
3. Keep under 500 lines (context window optimization)
4. Submit PR (required skills need RFC approval)
