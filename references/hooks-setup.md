# Hooks Setup (Optional — Advanced)

Hooks are shell commands Claude Code runs automatically at specific events. Useful for auto-loading vault context at session start, or auto-logging at session end.

**Warning:** hooks run on *every* session. A broken hook breaks every future session until fixed. Get the basics of vault usage down first; add hooks later.

## Where hooks live

Global (all projects): `~/.claude/settings.json`
Per-project: `<project>/.claude/settings.json`

Hook scripts: wherever you want, absolute paths recommended.

## Hook 1 — Session start (auto-load Active Tasks)

Injects `Active Tasks.md` into every new session's context, so Claude sees current work without being asked.

### Script: `~/.claude/hooks/session-start.sh`

```bash
#!/bin/bash
# Prints current Active Tasks for Claude session start.
# stdout gets injected into the session as context.

VAULT="/absolute/path/to/ObsidianVault"

echo "## Active Tasks"
echo ""
if [ -f "$VAULT/Memory/Active Tasks.md" ]; then
  cat "$VAULT/Memory/Active Tasks.md"
else
  echo "_No Active Tasks file found at $VAULT/Memory/Active Tasks.md_"
fi

echo ""
echo "## Recent Lessons"
echo ""
if [ -f "$VAULT/Memory/Lessons Learned.md" ]; then
  # Last 30 lines — recent lessons only
  tail -n 30 "$VAULT/Memory/Lessons Learned.md"
fi
```

Make executable:
```bash
chmod +x ~/.claude/hooks/session-start.sh
```

### `settings.json` entry

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "startup",
        "hooks": [
          {
            "type": "command",
            "command": "/absolute/path/to/.claude/hooks/session-start.sh"
          }
        ]
      }
    ]
  }
}
```

### Windows note

Use `.ps1` or `.cmd`, not `.sh`. Example PowerShell:
```powershell
# C:\Users\you\.claude\hooks\session-start.ps1
$vault = "C:\Users\you\ObsidianVault"
Write-Output "## Active Tasks`n"
if (Test-Path "$vault\Memory\Active Tasks.md") {
  Get-Content "$vault\Memory\Active Tasks.md"
}
```

And in settings:
```json
"command": "powershell -File C:\\Users\\you\\.claude\\hooks\\session-start.ps1"
```

## Hook 2 — Session stop (log + bridge update)

When a session ends, write a short bridge entry so the next session knows what just happened.

### Script: `~/.claude/hooks/session-stop.sh`

```bash
#!/bin/bash
# Append a stub to Session Log so Claude has a record.
# Claude fills in details via memory-extractor, or user edits manually.

VAULT="/absolute/path/to/ObsidianVault"
LOG="$VAULT/Memory/Session Log.md"
TS=$(date -u +"%Y-%m-%d %H:%M UTC")

# Append header only — actual content should be written during the session
echo "" >> "$LOG"
echo "## $TS — Session ended" >> "$LOG"
echo "_Fill in: context, done, decisions, next_" >> "$LOG"
```

### `settings.json` entry

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "/absolute/path/to/.claude/hooks/session-stop.sh"
          }
        ]
      }
    ]
  }
}
```

## Hook 3 — Audit trail (every vault write logged)

If you want to audit every file Claude writes to your vault:

```bash
#!/bin/bash
# ~/.claude/hooks/vault-write-log.sh
# Logs every write operation to vault/.audit.log

VAULT="/absolute/path/to/ObsidianVault"
AUDIT="$VAULT/.audit.log"
TS=$(date -u +"%Y-%m-%dT%H:%M:%SZ")

# Read the tool input from stdin
INPUT=$(cat)
FILE_PATH=$(echo "$INPUT" | grep -oE '"file_path":\s*"[^"]*"' | sed 's/"file_path":\s*"//' | sed 's/"$//')

if [[ "$FILE_PATH" == "$VAULT"* ]]; then
  echo "$TS WRITE $FILE_PATH" >> "$AUDIT"
fi
```

Trigger with `PostToolUse` matcher `Write|Edit`.

## Common mistakes

**Hook fails silently** — Claude doesn't surface hook errors clearly. Test hooks from the terminal first: `bash ~/.claude/hooks/session-start.sh` should produce output without errors.

**Hook output too large** — if your Active Tasks file is 50KB and you dump it all, you burn 15K tokens per session. Trim old/done items from Active Tasks aggressively.

**Encoding issues on Windows** — Cyrillic and other non-ASCII characters can break under CP1251. Force UTF-8: add `[Console]::OutputEncoding = [System.Text.Encoding]::UTF8` at the top of PowerShell scripts.

**Hook runs but vault write still fails** — Claude doesn't wait for your hook to finish editing the vault. If hook is long-running, use `&` to background it.

## When hooks are worth it

- You work in the vault-backed project **daily**
- You have recurring session patterns (standup, review, etc.)
- You want audit trail for compliance or review

## When to skip hooks

- You use the vault occasionally
- You're still iterating on structure (hooks assume stable paths)
- You can't easily debug shell scripts on your platform

Rule of thumb: **use the vault manually for a month, then add hooks for the patterns that emerged.**
