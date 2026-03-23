# CLAUDE.md Template

> Generic CLAUDE.md template for new projects. Replace all {{PLACEHOLDER}} values before use.

---

```markdown
# CLAUDE.md — {{PROJECT_NAME}}

{{PROJECT_NAME}} is [one-line product description].

## Tech Stack

{{TECH_STACK_TABLE}}

<!--
Example:
| Layer | Technology |
|-------|-----------|
| Backend API | FastAPI (async, Python 3.13) |
| Frontend | Next.js 15 (App Router, SSR/ISR, TypeScript) |
| Database | PostgreSQL 16 |
| ORM | SQLAlchemy 2.0 (async) + Alembic |
| Styling | Tailwind CSS 4 |
| Icons | Lucide React |
-->

## Project Structure

```
{{PROJECT_STRUCTURE}}
```

<!--
Example:
ProjectName/
├── backend/app/          # FastAPI (models/, schemas/, api/, services/)
├── frontend/src/         # Next.js 15 (app/, components/, lib/)
├── docs/                 # product/ + design/ + technical/
├── .claude/agents/       # Role definitions
├── .claude/rules/        # Path-triggered code rules
└── infra/                # Docker, Nginx configs
-->

## Development

### Setup

```bash
{{SETUP_COMMANDS}}
```

### Common Commands

```bash
{{COMMON_COMMANDS}}
```

## Architecture

[Describe key architectural decisions and patterns here.]

## Code Conventions

See `.claude/rules/` for path-triggered rules (backend.md, frontend.md, etc.).

## Design System

**All UI/frontend work MUST follow [`docs/design/design-system.md`](./docs/design/design-system.md).**

Single source of truth for colors, typography, spacing, components, and interaction patterns.

## Team Collaboration

**Team rules: [`docs/team-rules.md`](./docs/team-rules.md)**

### Team Management Goals

All decisions and actions must serve these four goals:

1. **Fast Iteration** — Rapid development and deployment of product features
2. **Efficient Collaboration** — Information sync with minimal communication overhead
3. **Autonomous Operation** — Team runs independently, founder only makes directional decisions
4. **Continuous Growth** — Experience crystallizes into rules and docs, team capability grows

### Agent Roles

Role definitions in `.claude/agents/` directory:

| Role | File | Model | Core Responsibility |
|------|------|-------|---------------------|
| Product Designer | `.claude/agents/product-designer.md` | Opus | Product requirements, design, interaction |
| CTO | `.claude/agents/cto.md` | Opus | Architecture, code review, deployment |
| Dev | `.claude/agents/dev.md` | Sonnet | Full-stack development, bug fixes |
| Tester | `.claude/agents/tester.md` | Sonnet | Independent testing, quality gates |

### Session Recovery

To recover project state in a new session:
1. Read `docs/board.md` — task overview
2. Read `docs/handoff.md` — last session context
3. Read `memory/` — relevant feedback and project memory

### Team Workflow and Collaboration

- **Workflow**: [`docs/team-workflow.md`](./docs/team-workflow.md) (10-step process + exception handling)
- **Doc Management**: [`docs/doc-management.md`](./docs/doc-management.md)

## ALWAYS

1. Read relevant docs before writing any code (requirements, design spec, tech plan)
2. Run verification commands after every change — never report complete without passing
3. Fix at root cause — never patch surface symptoms
4. Pass file paths between agents, not file contents
5. One source of truth — same information in only one place

## NEVER

- NEVER `git push --force` to main
- NEVER skip pre-commit hooks (`--no-verify`)
- NEVER change DB schema without migration
- NEVER modify `.pen` files outside Designer role
- NEVER change architecture/tech stack without founder approval
- NEVER run destructive production commands without verification
- NEVER mark task complete without passing verification
- NEVER modify `.env`, secrets, or credentials without founder approval
- NEVER trial-and-error on production systems — investigate first, act once
{{PROTECTED_FILES_RULES}}

<!--
Example protected files rules:
- NEVER modify database outside CTO role
- NEVER edit infra/ files without CTO approval
-->

## Verification (by task type)

### Frontend changes
```bash
{{FRONTEND_BUILD}}
{{FRONTEND_TYPECHECK}}
```

### Backend changes
```bash
{{BACKEND_CHECK}}
```

## Design References

{{DESIGN_REFS}}

<!--
Example:
- `.pen` file: `docs/design/project-ui.pen` (唯一设计稿)
- Design system: `docs/design/design-system.md`
-->

## Brand & Design

**Brand**: [Brand personality description]
**Design spec**: [`docs/design/design-system.md`](./docs/design/design-system.md)
**.pen file**: `docs/design/project-ui.pen`

## Compact Instructions

When context is compressed, MUST preserve:
1. Current task board state and in-progress items
2. Architecture decisions made in this session
3. Modified files list and nature of changes
4. Verification results (test pass/fail, build status)
5. Open risks, blockers, and pending approvals
6. VPS/deployment state changes
7. Team member status (who is active, who completed what)
```
