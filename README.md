# Ariadne Thread

> Structure codebases so AI agents can navigate, locate, and safely modify code — without reading the entire project.

[中文版](README.zh-CN.md)

---

## What it does

Ariadne Thread creates a layered index system that gives AI agents a clear path from project overview down to implementation detail. Like Ariadne's thread through the labyrinth, it lets an agent understand structure, find any function, and assess modification impact — at a fraction of the token cost of reading raw code.

**Measured effect**: in structured evaluations, outputs produced with this skill passed **100% of AI-navigation assertions** vs. 59% without it. The biggest gaps without the skill: missing Modification Risk analysis, Task Routing maps, Cross-Cutting Patterns documentation, and Agent Workflow checklists — the sections with the highest token-saving leverage.

---

## The 4-Level Index

```
L0  AGENTS.md          Project root — map, navigation table, build commands,
│                       architectural constraints, cross-cutting patterns,
│                       Agent Workflow checklist (80–150 lines)
│
L1  INDEX.md           One per module directory — public API, dependencies,
│                       dependents (fan-in), modification risk, task routing,
│                       interface contracts, test locations (~50 lines)
│
L2  File headers       5–10 lines per source file — intent, public interface,
│                       contracts (@pre/@post/@throws), side effects
│
L3  Inline comments    Non-obvious logic only — the why, not the what
```

Agents start at L0, drill down only as far as the task requires.

---

## Key differentiators vs. basic documentation

| Section | AI question answered | Token saving |
|---------|---------------------|-------------|
| **Dependents (Fan-in)** | Who calls me? What breaks if I change this? | Avoids full-project grep to discover callers |
| **Modification Risk** | Is this change safe? How many files to update? | Avoids underestimating blast radius |
| **Task Routing** | Which file do I edit for task X? | Avoids reading the wrong files before finding the right one |
| **Interface Contract** | What invariants must I preserve? | Avoids re-reading callers to infer constraints |
| **Cross-Cutting Patterns** | How does this project handle errors/logging/concurrency? | Avoids reading 3–4 source files to infer project-wide patterns |

---

## When to use

**Use this skill for:**
- Starting a new project and want AI-friendly structure from day one
- Retrofitting an existing codebase with navigation indexes
- Creating or updating `AGENTS.md`, `INDEX.md`, or `llms.txt`
- Designing module boundaries with explicit dependency and modification-risk documentation
- Making a codebase navigable for AI agents without reading the whole thing

**Skip it when:**
- Project has fewer than ~5 source files (a lightweight `AGENTS.md` is enough)
- Quick throwaway prototype
- Single-layer codebase with no cross-module calls

---

## How to invoke

> **Important**: This skill covers a task Claude can often attempt on its own, so it may not auto-trigger. For best results, mention it explicitly.

**One-off:**
```
use the ariadne-thread skill to create indexes for this project
```

**Permanent (recommended)** — add to your project's `CLAUDE.md`:
```markdown
## AI Navigation
When creating or updating AGENTS.md, INDEX.md, or any project navigation
index, use the ariadne-thread skill.
```

This ensures the skill is always consulted for index work, regardless of how the request is phrased.

---

## Supported languages

C++, TypeScript/JavaScript, Python, Go, Rust, Java/Kotlin — with per-language conventions for directory layout, file naming, L2 contract annotations, and build commands. See [`references/language-adaptation.md`](references/language-adaptation.md).

Works for compiled libraries, header-only libraries, web applications, monorepos, and sub-projects.

---

## Quick start

**New project:**
1. Identify language ecosystem → see [`references/language-adaptation.md`](references/language-adaptation.md)
2. Design directory structure and module boundaries
3. Create `AGENTS.md` (all 9 sections — none are optional)
4. Create `INDEX.md` for each module as you create each directory
5. Add file intent headers (L2); public API files need contract annotations

**Existing project (retrofit):**
1. Index first, refactor later — document AS-IS, flag issues with ⚠
2. Start with the highest Fan-in module (most depended-on = most navigation value)
3. Create L0 (`AGENTS.md`) → L1 (`INDEX.md` per module) → L2/L3
4. Flag files with forbidden names (`utils.*`, `helpers.*`, `types.*`) with `⚠ needs split`

Full step-by-step workflows in [SKILL.md](SKILL.md).

---

## File structure

```
ariadne-thread/
├── SKILL.md                      # Main skill instructions
├── README.md                     # This file (English)
├── README.zh-CN.md               # Chinese version
└── references/
    ├── index-templates.md        # Copy-paste L0/L1/L2 templates
    ├── index-maintenance.md      # Tier A/B maintenance trigger table
    ├── language-adaptation.md    # Per-language conventions
    ├── naming-api-conventions.md # Naming rules and API design patterns
    └── doc-standards.md          # Human-facing doc standards and ADR format
```

---

## Documentation tiers

| Tier | Files | Update timing | Staleness impact |
|------|-------|--------------|-----------------|
| **A — AI runtime index** | `AGENTS.md`, `INDEX.md`, file headers | Real-time, atomic with code changes | Critical — stale = wrong navigation |
| **B — Human docs** | `README.md`, `architecture.md`, ADRs, API docs | Batch at iteration end | Moderate — humans notice, AI unaffected |

Keeping these tiers separate reduces per-change AI overhead (the exact amount scales with project size) and improves Tier B quality (batch updates are more coherent).
