# Example `CLAUDE.md` — Obsidian Vault Integration

This is the file Claude Code reads at the start of every session in a project.
Put this in the root of any project folder where you want Claude to use the vault as memory.

Replace `<VAULT_PATH>` with your actual absolute vault path.

---

```markdown
# Project: <Your Project Name>

## Memory — Obsidian Vault

- **Vault path:** `<VAULT_PATH>`  (e.g. `/Users/you/ObsidianVault` or `C:\Users\you\ObsidianVault`)
- **Entry point:** `<VAULT_PATH>/00-INDEX.md` ← read this FIRST on every session start
- **Active tasks:** `<VAULT_PATH>/Memory/Active Tasks.md`
- **Lessons learned:** `<VAULT_PATH>/Memory/Lessons Learned.md`

## Memory Protocol

### Session start
1. Read `00-INDEX.md` for vault structure
2. Read `Memory/Active Tasks.md` for current work
3. Read the most recent `Memory/Daily Reports/*.md` if asked about "yesterday"

### During work — save without asking:
- New person → `People/{Name}.md` + update `People/Clients MOC.md` if client
- Business decision → `Business/{topic}.md`
- Technical decision → `Projects/{project}.md`
- New operational rule → `Rules/{rule}.md`
- Mistake + conclusion → append to `Memory/Lessons Learned.md`

### Session end — always:
1. Update `Memory/Active Tasks.md` (prune done, add new)
2. Append short entry to `Memory/Session Log.md`
3. If context is running low → write progress to Active Tasks IMMEDIATELY

## Communication
- Language: <en / ua / ru / your language>
- Tone: <direct / formal / casual>
- Never save secrets to the vault (use a password manager)

## Extra rules
- Prefer editing existing vault files over creating new ones
- Link everything: `[[Person Name]]`, `[[Project Name]]` — Obsidian graph depends on this
- Don't use emojis in vault content unless the status icons (🟡🟢🔴🔥⚪)
```

---

## Notes

- **Where to place this file?**
  - If your project folder IS the vault → put `CLAUDE.md` in the vault root.
  - If vault is separate → put `CLAUDE.md` in each project folder that should have access.
  - If you want it everywhere → also copy into `~/.claude/CLAUDE.md` (global).

- **Vault path gotchas:**
  - Use forward slashes even on Windows: `C:/Users/you/ObsidianVault`
  - Quote paths with spaces: `"/Users/you/My Vault"`
  - Absolute, never relative

- **Don't overload this file.** It's loaded every session. Keep it under 100 lines. Details go in the vault itself.
