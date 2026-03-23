# Claude Code Hooks Reference

> Hook patterns for new projects. Replace all {{PLACEHOLDER}} values before use.
> Place final config in `.claude/settings.json` under the `"hooks"` key.

---

## Hook Configuration Structure

```json
{
  "hooks": {
    "PreToolUse": [...],
    "PostToolUse": [...]
  }
}
```

---

## PreToolUse / Bash — Force-push block

Prevents accidental force-push to main/master.

```json
{
  "matcher": "Bash",
  "hooks": [
    {
      "type": "command",
      "command": "input=$(cat); cmd=$(echo \"$input\" | python3 -c \"import sys,json; d=json.load(sys.stdin); print(d.get('command',''))\" 2>/dev/null || echo ''); if echo \"$cmd\" | grep -qE 'git push.*(--force|-f).*main|git push.*(--force|-f).*master'; then echo 'BLOCKED: force-push to main/master is forbidden' >&2; exit 2; fi | head -5"
    }
  ]
}
```

---

## PreToolUse / Bash — No-verify block

Prevents skipping pre-commit hooks.

```json
{
  "matcher": "Bash",
  "hooks": [
    {
      "type": "command",
      "command": "input=$(cat); cmd=$(echo \"$input\" | python3 -c \"import sys,json; d=json.load(sys.stdin); print(d.get('command',''))\" 2>/dev/null || echo ''); if echo \"$cmd\" | grep -qE 'git commit.*--no-verify|git commit.*-n '; then echo 'BLOCKED: --no-verify is forbidden. Fix the hook failure instead.' >&2; exit 2; fi | head -5"
    }
  ]
}
```

---

## PreToolUse / Bash — Build verification gate

Runs verification before allowing deployment commands. Replace {{PROJECT_DIR}} and {{FRONTEND_BUILD_CMD}} / {{BACKEND_CHECK_CMD}}.

```json
{
  "matcher": "Bash",
  "hooks": [
    {
      "type": "command",
      "command": "input=$(cat); cmd=$(echo \"$input\" | python3 -c \"import sys,json; d=json.load(sys.stdin); print(d.get('command',''))\" 2>/dev/null || echo ''); if echo \"$cmd\" | grep -qE 'git push|docker.*deploy|gh workflow run'; then cd {{PROJECT_DIR}} && {{FRONTEND_BUILD_CMD}} 2>&1 | head -20 && {{BACKEND_CHECK_CMD}} 2>&1 | head -10 || { echo 'BUILD FAILED — fix before deploying' >&2; exit 2; }; fi"
    }
  ]
}
```

**Placeholders:**
- `{{PROJECT_DIR}}` — absolute path to project root, e.g. `/Users/you/Projects/MyApp`
- `{{FRONTEND_BUILD_CMD}}` — e.g. `cd frontend && npm run build`
- `{{BACKEND_CHECK_CMD}}` — e.g. `cd backend && uv run python -c "from app.main import app"`

---

## PreToolUse / Edit|Write — Protected files block

Prevents editing files that should only be modified by specific roles. Replace {{PROTECTED_PATTERN}}.

```json
{
  "matcher": "Edit|Write",
  "hooks": [
    {
      "type": "command",
      "command": "input=$(cat); path=$(echo \"$input\" | python3 -c \"import sys,json; d=json.load(sys.stdin); print(d.get('file_path',''))\" 2>/dev/null || echo ''); if echo \"$path\" | grep -qE '{{PROTECTED_PATTERN}}'; then echo \"BLOCKED: $path is a protected file. Only the designated role may modify it.\" >&2; exit 2; fi | head -5"
    }
  ]
}
```

**Placeholders:**
- `{{PROTECTED_PATTERN}}` — regex pattern for protected files, e.g. `\\.pen$|\\.env$|infra/`

**Example patterns:**

| What to protect | Pattern |
|----------------|---------|
| .pen design files | `\\.pen$` |
| Environment files | `\\.env(\\..*)?$` |
| Infrastructure configs | `^infra/` |
| Migration files | `alembic/versions/` |
| All of the above | `\\.pen$|\\.env(\\..*)?$|^infra/|alembic/versions/` |

---

## PostToolUse / Edit|Write — Type checking

Runs TypeScript type check after editing frontend files. Truncated output prevents context bloat.

```json
{
  "matcher": "Edit|Write",
  "hooks": [
    {
      "type": "command",
      "command": "input=$(cat); path=$(echo \"$input\" | python3 -c \"import sys,json; d=json.load(sys.stdin); print(d.get('file_path',''))\" 2>/dev/null || echo ''); if echo \"$path\" | grep -qE '\\.(ts|tsx)$' && echo \"$path\" | grep -q 'frontend/'; then cd {{PROJECT_DIR}}/frontend && npx tsc --noEmit 2>&1 | head -30; fi"
    }
  ]
}
```

---

## Combined settings.json example

Full example for a Next.js + FastAPI project:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "input=$(cat); cmd=$(echo \"$input\" | python3 -c \"import sys,json; d=json.load(sys.stdin); print(d.get('command',''))\" 2>/dev/null || echo ''); if echo \"$cmd\" | grep -qE 'git push.*(--force|-f).*(main|master)'; then echo 'BLOCKED: force-push to main/master is forbidden' >&2; exit 2; fi | head -5"
          },
          {
            "type": "command",
            "command": "input=$(cat); cmd=$(echo \"$input\" | python3 -c \"import sys,json; d=json.load(sys.stdin); print(d.get('command',''))\" 2>/dev/null || echo ''); if echo \"$cmd\" | grep -qE 'git commit.*(--no-verify|-n )'; then echo 'BLOCKED: --no-verify is forbidden' >&2; exit 2; fi | head -5"
          }
        ]
      },
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "input=$(cat); path=$(echo \"$input\" | python3 -c \"import sys,json; d=json.load(sys.stdin); print(d.get('file_path',''))\" 2>/dev/null || echo ''); if echo \"$path\" | grep -qE '\\.pen$|\\.env(\\..*)?$'; then echo \"BLOCKED: $path is a protected file\" >&2; exit 2; fi | head -5"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "input=$(cat); path=$(echo \"$input\" | python3 -c \"import sys,json; d=json.load(sys.stdin); print(d.get('file_path',''))\" 2>/dev/null || echo ''); if echo \"$path\" | grep -qE '\\.(ts|tsx)$' && echo \"$path\" | grep -q 'frontend/'; then cd {{PROJECT_DIR}}/frontend && npx tsc --noEmit 2>&1 | head -30; fi"
          }
        ]
      }
    ]
  }
}
```

---

## Notes

- All hook commands must include output truncation (`| head -N`) to prevent context bloat.
- Use `exit 2` (not `exit 1`) to block the tool call — exit 2 signals a hard block to Claude Code.
- Hook JSON must be valid — test with `python3 -m json.tool .claude/settings.json`.
- Hooks run in the shell environment of the user's profile; ensure tools like `npx`, `uv` are on PATH.
- Keep each hook command under 500 characters when possible — long commands are harder to audit.
