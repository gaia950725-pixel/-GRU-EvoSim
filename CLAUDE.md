# CLAUDE.md — EvoSim AI Assistant Reference

> **Current Version**: v27.04 (Season Spatial Amplitude)
> **Last Updated**: 2026-03-02

This file is the primary reference for AI assistants (Claude, Codex, etc.) working on this repository. For policy charter and philosophy see `docs/governance/MASTER_PROMPT.md`; for GitHub workflow see `docs/ops/PROJECT_OPERATIONS.md`; for output format specs see `docs/spec/SPEC_OUTPUTS.md`.

---

## Project Overview

**EvoSim: Neural Evolution Simulator** — A browser-based educational/research simulator where GRU neural network–driven agents evolve through genetic algorithms and cultural transmission. Multiple clans compete across procedurally generated terrain (Neolithic → Ancient eras), exhibiting emergent behaviors including resource competition, territorial warfare, and social hierarchy formation.

**Delivery format**: Single-file HTML (~18,300 lines, ~750 KB). The entire application — styles, DOM, and all JavaScript — is bundled into one HTML artifact.

---

## Repository Layout

```
/
├── index.html                        # Dev runtime entry point (keep in sync with latest)
├── package.json                      # npm scripts: dev, build, preview
├── tsconfig.json                     # TypeScript config (ES2022, no emit)
├── vite.config.ts                    # Dev server on :3000, GEMINI_API_KEY injection
├── metadata.json                     # Version/release tracking metadata
├── README.md                         # Quick-start and canonical file index
│
└── docs/
    ├── governance/
    │   └── MASTER_PROMPT.md          # Policy charter (update section [7] only for state)
    ├── ops/
    │   └── PROJECT_OPERATIONS.md     # Branch strategy, PR gates, artifact routing
    ├── ai/
    │   └── AI_HANDOFF_PROTOCOL.md    # Task handoff format for AI execution teams
    ├── spec/
    │   └── SPEC_OUTPUTS.md           # Required fields per output type
    └── releases/
        ├── EvoSim_latest.html        # Canonical latest snapshot (ALWAYS keep in sync)
        ├── EvoSim_CHANGELOG.txt      # Append-only changelog (v26.44+, newest first)
        └── archive/
            ├── EvoSim_27.03_maritime_sot_FINAL.html
            └── EvoSim_27.04_season_spatial_temp_crop_FINAL.html
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Runtime | HTML5 Canvas 2D (single-file delivery) |
| Language | TypeScript ~5.8 (type-checking only; `noEmit: true`) |
| Build | Vite 6.2 (`npm run dev` → port 3000) |
| ML | Custom `GRUNetwork` class (pure JS matrix algebra) |
| External API | Google Gemini (`@google/genai`) — requires `GEMINI_API_KEY` |
| Module system | ES modules (`"type": "module"`) |

**No test framework** — verification is manual (in-browser simulation runs + console checks).

---

## Development Workflow

### Local Setup

```bash
npm install
# Create .env.local with: GEMINI_API_KEY=<your_key>
npm run dev       # Dev server at http://localhost:3000
npm run build     # Bundle to dist/
npm run preview   # Preview built output
```

### Branch Conventions

Branches follow: `release/<version>-<task>`, `fix/<task>`, `docs/<task>`, `feature/<task>`

- All work for the same version number accumulates in **one branch** (no new branch per commit).
- Version-number updates must always use `release/<version>-<task>` (do not use `feature/*` for version bumps).
- Independent version lines (e.g., 27.04 vs a parallel experiment) must **never be cross-merged** — use cherry-pick only with explicit user approval.
- Version-bump integration order: `release/*` → operative branch → `main`

**AI 실행팀 브랜치**: Claude·Codex 모두 동일한 컨벤션(`feature/*`, `fix/*`, `docs/*`, `release/*`)을 따른다. 버전 갱신 작업은 예외 없이 `release/*`를 사용한다. 실행 환경이 자동 배정하는 세션 브랜치(`claude/<slug>`, `codex/<slug>`)는 세션 전용이므로 이 파일에 특정 브랜치명을 기재하지 않는다. 현재 작업 브랜치는 시스템 프롬프트를 확인한다.

**Ops-only / docs-only changes (no sim version bump)**: commit directly to the current AI working branch. No separate `docs/<task>` branch is required when the change is purely operational (e.g., filling CHANGELOG templates, updating governance docs) and carries no code behavior change.

### Commit Message Style

```
[<version>][P<n>/<total>] short description
```
Examples:
- `[27.02][P1/4] season core`
- `[27.02][P2/4] locale debug`
- `feat: apply 27.04 season spatial amp prompts and finalize release artifacts`

### Merge Gate (Definition of Done)

Before any PR is marked ready:
1. Main HTML (`index.html` or designated single-file runtime) is updated
2. `docs/releases/EvoSim_latest.html` synchronized with the runtime HTML
3. Versioned archive snapshot created: `docs/releases/archive/EvoSim_<version>_<patch>_FINAL.html`
4. `docs/releases/EvoSim_CHANGELOG.txt` updated (for intermediate/critical patches)
5. Execution report follows the fixed 4-section format in `docs/spec/SPEC_OUTPUTS.md`
6. Version-bump branch naming rule (`release/*`) is satisfied
7. User has verified gameplay/console (manual play test, no console errors)
8. No independent-line policy violations

---

## Editing Rules — BUILDER_BLOCK System

The entire HTML file is segmented into named blocks:

```html
<!-- === BUILDER_BLOCK: BLOCK_NAME_START === -->
  ... code ...
<!-- === BUILDER_BLOCK: BLOCK_NAME_END === -->
```

**Rules**:
- All edits must stay within the correct BUILDER_BLOCK boundaries.
- Cross-block changes require explicit justification in the commit/PR.
- Prefer diff-based edits over full-file replacement.
- Never re-transmit the whole file when only a block changed.

**Key blocks** (sampled):
`STYLES_START`, `CSS_START`, `UI_DOM_START`, `HOME_OVERLAY_START`, `CONFIG_OVERLAY_START`, `UI_INGAME_START`, `ARCHITECTURE_OVERVIEW`, `JS_TOP`, `JS_BOOTSTRAP_CORE`, `SURNAME_AUTO_CORE`, `WORLDGEN_UI_INIT`

---

## Code Conventions

### General
- **Classes/functions**: PascalCase (`GRUNetwork`, `AgentGrid1D`)
- **Constants**: UPPER_CASE (`AGENT_CELL`, `SEASON_AMP_COAST`)
- **Variables**: snake_case
- **Global config singleton**: `CFG` object (mutable, holds all tuning parameters)
- **2D math**: `Vec2` class; **NN weights**: `Matrix` class
- **Event handling**: `__createEvoEventBus()` event bus for decoupled communication

### Comments — Minimal by Design
The CHANGELOG is the source of truth for history and intent. Inline comments are restricted:

| Allowed | Prohibited |
|---|---|
| `// RENDER-ONLY GUARD` | Phase/step notes |
| `// [DEPRECATED]` | Stale version comments |
| `// LOCAL INVARIANT` | Magic-number justifications |

Do not add comments unless they match one of the allowed categories.

### Performance Principles
- **Cache-first**: No per-frame full-world scans. Use caches + low-frequency refresh (0.25–1 s) + event-driven invalidation.
- **Grid systems**: `AgentGrid1D`, `FoodGrid1D`, `HerbGrid1D`, `PredGrid1D`, `FastIdMap` for spatial lookups.
- **Tick pipeline**: Pre-snapshot → frame compute → commit writes. Reads and writes must not be interleaved.
- **Fail-fast**: The core simulation loop must error visibly rather than silently degrade.

### Neural Network I/O Contract (v27.00+)
The GRU network has **27 input slots**:

| Sense | Slots | Count |
|---|---|---|
| Vision | 0–7 | 8 |
| Audition | 8–11 | 4 |
| Olfaction | 12–14 | 3 |
| Proprioception | 15–17 | 3 |
| Interoception | 18–22 | 5 |
| Sociality | 23–26 | 4 |

Any change to this schema is a **critical/structural** change and must be recorded in `ARCHITECTURE_OVERVIEW` + `PATCH RECORD` + `CHANGELOG`.

---

## Artifact Routing & Change Classification

### Importance Levels

| Level | When | Required Records |
|---|---|---|
| **Minor** | Small tuning, no behavioral change | None (or inline comment only) |
| **Intermediate** | New behavior, balance change | `EvoSim_CHANGELOG.txt` |
| **Critical** | Pipeline/schema/NN I/O/large refactor | `ARCHITECTURE_OVERVIEW` + `PATCH RECORD` + `CHANGELOG` |

Structural changes (TS/tick structure, pipeline order, inter-system dependencies, NN I/O) always qualify as **critical**.
When classification is ambiguous, escalate to the user as a policy request.

### File Update Mapping

| Change type | Files to update |
|---|---|
| Any code behavior change | `index.html` + `docs/releases/EvoSim_latest.html` + archive snapshot |
| Intermediate/critical patch | Above + `docs/releases/EvoSim_CHANGELOG.txt` |
| Governance policy change | `docs/governance/MASTER_PROMPT.md` (`last update` field) |

### Path Convention
Always use **repository-relative paths** in prompts and outputs. Absolute paths (`/mnt/data/…`, `/home/user/…`) are prohibited.

---

## CHANGELOG Format

File: `docs/releases/EvoSim_CHANGELOG.txt` — append-only, newest entries at the top.

Each entry must cover:
1. What changed
2. Why it changed
3. Regression risk
4. How to verify

---

## Responsibility Split

| Role | Responsibility |
|---|---|
| **User** | Policy decisions, final merge approval, gameplay verification |
| **Planning team** (ChatGPT/Gemini) | Work specs, blueprints, quality risk review, importance classification |
| **Execution team** (Claude/Codex) | Branch/commit/PR creation, code patching, FINAL output sync |

---

## Standard Process Flows

**Full process**: Idea meeting → Work roadmap → Code survey → Blueprint → Spec → Policy confirmation (user) → Patch roadmap → Work spec → Prompt → Execute → Execution report → Code review → User review

**Express process** (user-requested): Idea meeting → Prompt → Patch execution → User review

---

## AI Execution Checklist

Before closing any task:

- [ ] Edits confined to correct BUILDER_BLOCK(s)
- [ ] `index.html` updated
- [ ] `docs/releases/EvoSim_latest.html` synchronized
- [ ] Archive snapshot created (`docs/releases/archive/EvoSim_<version>_<patch>_FINAL.html`)
- [ ] CHANGELOG updated if intermediate or critical
- [ ] ARCHITECTURE_OVERVIEW / PATCH RECORD updated if critical/structural
- [ ] Commit message follows `[version][Pn/total] description` style
- [ ] All paths in outputs are repository-relative
- [ ] No prohibited comment types added
- [ ] No cross-line branch merges without user approval
