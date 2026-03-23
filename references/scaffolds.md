# Document Scaffolds

> Copy-paste templates for standard project documents. Replace {{PROJECT_NAME}} and {{DATE}}.

---

## board.md

```markdown
---
title: Task Board
owner: Lead
updated: {{DATE}}
---

# {{PROJECT_NAME}} — Task Board

## In Progress

| Task | Owner | Started | Notes |
|------|-------|---------|-------|
| | | | |

## Queued (Prioritized)

| Priority | Task | Type | Blocked By |
|----------|------|------|------------|
| P1 | | | |
| P2 | | | |

## Completed (this sprint)

| Task | Completed | Notes |
|------|-----------|-------|
| | | |

## Backlog

| Task | Type | Notes |
|------|------|-------|
| | | |

## Blocked

| Task | Blocked By | Since |
|------|------------|-------|
| | | |

---

## Legend

| Type | Meaning |
|------|---------|
| feat | New feature |
| fix | Bug fix |
| chore | Maintenance / docs |
| infra | Infrastructure / deployment |

| Priority | Meaning |
|----------|---------|
| P1 | Must ship this session |
| P2 | Should ship this sprint |
| P3 | Nice to have |
```

---

## handoff.md

```markdown
---
title: Session Handoff
owner: Lead
updated: {{DATE}}
---

# Session Handoff — {{DATE}}

## Completed This Session

- [specific deliverable with file paths]

## Attempted but Not Completed

- [what was tried, why it failed or was stopped]

## Current Blockers

- [what is blocking, who is responsible, what is needed]

## Start Here Next Session

1. [first action — be specific]
2. [second action]
3. [third action]

## Active Decisions Made This Session

- [decision and rationale — reference decisions.md for full record]

## Files Modified This Session

- `path/to/file.ts` — [what changed and why]
- `path/to/file.py` — [what changed and why]

## Verification Status

- [ ] Frontend build: PASS / FAIL
- [ ] TypeScript typecheck: PASS / FAIL
- [ ] Backend import check: PASS / FAIL
- [ ] Tests: PASS / FAIL / N/A
```

---

## decisions.md

```markdown
---
title: Product Decisions & Vision
owner: Product Designer
updated: {{DATE}}
---

# {{PROJECT_NAME}} — Product Decisions & Vision

> Append-only. Never modify past entries. Add new entries at the bottom.

## Product Vision

[1-2 paragraph product vision statement]

## North Star Metrics

- [primary metric]
- [secondary metric]

---

## Decision Log

### {{DATE}} — [Decision Title]

**Context:** [Why this decision was needed]

**Decision:** [What was decided]

**Rationale:** [Why this option over alternatives]

**Alternatives considered:**
- [Alternative A] — rejected because [reason]
- [Alternative B] — rejected because [reason]

**Impact:** [What this affects]

**Owner:** [Who made the call]

---

<!-- New decisions go below this line, newest at bottom -->
```

---

## Usage Notes

- **board.md** is the cross-session task source of truth. Read at session start, update at session end.
- **handoff.md** is overwritten each session — it captures only the current session's context.
- **decisions.md** is append-only — never modify or delete past entries. Date each entry.
- All three files live in `docs/` and are committed to git so they persist across machines and agents.
- Keep board.md concise — detailed context lives in requirement/design/tech docs, not the board.
