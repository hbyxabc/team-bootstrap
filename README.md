# Team Bootstrap

> One command to set up a complete AI agent team for any project.

## What it does

`/team-bootstrap` creates a full AI agent team management system in your project:

- **4 Agent Roles** — Product Designer, CTO, Dev, Tester, each with defined responsibilities and tool permissions
- **10-Step Workflow** — From requirements to deployment, with verification gates at every step
- **Document Management** — Lifecycle rules, naming conventions, and living spec documents
- **Safety Hooks** — Automatic blocking of force-push, credential edits, and build verification before push
- **Session Continuity** — Handoff docs and memory system for cross-session context

## Quick Start

### Install

```bash
# From GitHub
npx skills add your-github/team-bootstrap -s team-bootstrap -g -y

# Or copy into your project
cp -r team-bootstrap/ /your-project/.claude/skills/team-bootstrap/
```

### Use

Open Claude Code in your project directory, then:

```
/team-bootstrap MyProjectName
```

Claude will ask you a few questions (tech stack, build commands, design tool), then generate everything automatically.

## What gets created

```
your-project/
├── CLAUDE.md                          # Project contract (~150 lines)
├── .claude/
│   ├── agents/
│   │   ├── product-designer.md        # CPO + PM + Designer
│   │   ├── cto.md                     # Architecture + Deploy + Review
│   │   ├── dev.md                     # Full-stack development
│   │   └── tester.md                  # Independent testing (read-only)
│   ├── rules/                         # (empty, add your own)
│   └── settings.json                  # Protective hooks
├── docs/
│   ├── board.md                       # Task board
│   ├── handoff.md                     # Session handoff
│   ├── team-workflow.md               # 10-step dev process
│   ├── doc-management.md              # Document lifecycle system
│   ├── product/
│   │   ├── decisions.md               # Decision record
│   │   ├── modules/                   # [Spec] docs per module
│   │   ├── requirements/              # [Requirement] docs per change
│   │   └── archive/
│   ├── design/                        # (if design tool selected)
│   │   ├── deliverables/
│   │   └── archive/
│   └── technical/
│       ├── deliverables/              # [Plan] and [Review] docs
│       └── archive/
```

## The 4 Roles

| Role | Model | What they do | What they can't do |
|------|-------|-------------|-------------------|
| **Product Designer** | Opus | Requirements, product logic, UX copy, visual design | Write code, deploy |
| **CTO** | Opus | Architecture review, code review, deployment, tech plans | Product decisions, visual design |
| **Dev** | Sonnet | Full-stack development, local testing, bug fixes | Modify design files, push to production |
| **Tester** | Sonnet | Independent testing, requirement verification | Modify any files (read-only) |

## The 10-Step Workflow

```
[1] Requirements → [2] Design → [3] Tech Review → [4] Development →
[5] Testing → [6] Code Review → [7] Acceptance → [8] Deploy →
[9] Production Verify → [10] Archive & Learn
```

- Bug fixes use a **fast track**: [1] → [4] → [6] → [8] → [9]
- Founder involvement at only 2 points: after [2] and at [7]

## Safety Hooks

Automatically enforced via `.claude/settings.json`:

| Hook | What it blocks |
|------|---------------|
| Force-push guard | `git push --force` or `-f` |
| No-verify guard | `--no-verify` flag |
| Build gate | Push without passing frontend build + backend check |
| File protection | Editing `.env`, credentials, or design files without permission |
| Type checker | Runs tsc/py_compile after every edit (catches errors immediately) |

## Design Tool Support

During setup, you can choose:

- **Pencil** (.pen files) — Auto-configures Pencil MCP. No API key needed.
- **Figma** — Guides you through getting a Figma API key and configures the connection.
- **None** — Skips design tool setup. Design-related sections are removed.

## After Setup

1. **Review CLAUDE.md** — Customize the ALWAYS/NEVER rules for your project
2. **Add code rules** — Create `.claude/rules/backend.md`, `frontend.md` etc. with language-specific conventions
3. **Create spec docs** — For each existing module, create a `[Spec]` doc in `docs/product/modules/`
4. **Try a small feature** — Run one feature through the full 10-step workflow to validate

## Key Concepts

### Spec Documents ([规格])

Each module has one living, complete spec document. It's not a summary — it's the full product definition. When a feature ships, requirement details merge into the spec verbatim. Read one spec, understand the entire module.

### Session Continuity

At session start: read `board.md` → `handoff.md` → `memory/`

At session end: update `board.md` + `handoff.md` → commit + push

### Document Lifecycle

```
draft → review → approved → implemented → archived
```

Requirement docs get archived after their content merges into the spec. Review docs get archived 7 days after conclusions land in code/rules.

## Requirements

- [Claude Code](https://claude.com/claude-code) CLI
- Node.js (for npx)
- Git

## License

MIT
