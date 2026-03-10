# Ariadne Thread

> Guide creation of AI-friendly project structures with progressive disclosure indexing, modular architecture, and intent-oriented code — enabling AI agents to navigate codebases efficiently.

## Overview

Ariadne Thread helps you structure codebases so AI agents can understand and safely modify them. Like Ariadne's thread through the labyrinth, it provides a clear path from project overview to implementation detail, enabling AI to:

- **Understand project structure**
- **Locate any function**
- **Assess the impact of any modification**

— without reading the entire codebase.

## Core Goals

| Goal | Description |
|------|-------------|
| **Navigation efficiency** | Minimize tokens consumed to find the right code |
| **Modification safety** | Maximize probability of correct changes via explicit contracts and dependency visibility |

## 4-Level Index Architecture

```
L0  Project Root    AGENTS.md (~80 lines)
│                   Project map, navigation table, build commands, cross-cutting patterns
│
L1  Module Index   INDEX.md per directory (~50 lines)
│                   Purpose, public API, dependencies, contracts, task routing, modification risk, test mapping
│
L2  File Intent     File headers (~5–10 lines per file)
│                   What this file does, public interface, contracts, side effects
│
L3  Inline Detail   Comments on non-obvious logic only
                    Why, not what; tricky edge cases; business rules
```

AI agents start at L0 and drill down only as needed.

## Features

- **Language-agnostic**: TypeScript, C++, Python, Go, Rust, Java, and more
- **Progressive disclosure**: Reveal only what's needed when it's needed, reducing token consumption
- **Dual documentation tiers**: Tier A (AI runtime index) and Tier B (human docs) maintained separately
- **Module design principles**: Single responsibility, clear dependency direction, small focused files

## Use Cases

- Starting a new project with AI-friendly structure
- Restructuring or indexing an existing codebase
- Adding project navigation indexes (AGENTS.md, INDEX.md, llms.txt)
- Designing module boundaries and dependencies
- Creating or maintaining codebase documentation for AI agents

## Trigger Keywords

Use this Skill when the user mentions:

- "AI-friendly project"
- "progressive disclosure"
- "project index"
- "codebase navigation"
- "ariadne"

## Document Structure

```
ariadne-thread/
├── SKILL.md                    # Full skill guide (4-level index, language adaptation, workflows)
├── README.md                   # This file (English)
├── README.zh-CN.md             # Chinese version
└── references/
    ├── index-templates.md      # Copy-paste L0/L1/L2 templates
    ├── naming-api-conventions.md # Naming and API conventions
    └── doc-standards.md        # Documentation standards and ADR format
```

## Quick Start

**New project:**

1. Design directory structure and module boundaries
2. Create `AGENTS.md` (project overview, navigation table, cross-cutting patterns)
3. Create `INDEX.md` for each module
4. Add file intent headers (L2); public API files need contract annotations

**Existing project:**

1. Index first, refactor later
2. Start with the highest Fan-in module (most depended-on)
3. Create L0 → L1, then add L2/L3

See the Workflow sections in [SKILL.md](SKILL.md) for detailed steps.

## License

As per project repository conventions.
