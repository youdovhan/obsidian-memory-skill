# Memory Architecture — Why 3 Tiers

## The problem this solves

Without a structured memory, Claude's useful output evaporates at the end of every session. You re-explain context each time. Decisions get lost. Clients turn into "I think I talked to someone about X last month."

A flat dump (one big `memory.md`) fails the other way — it grows until Claude can't find anything in it.

The fix: **three tiers with clear boundaries**.

## The tiers

### Tier 1 — Fast memory (Claude's built-in)

**Location:** `~/.claude/projects/<project>/memory/` (auto-managed by Claude)

**What belongs here:**
- Behavioral corrections ("don't add emojis", "always use `pnpm` not `npm`")
- User profile (role, expertise level, preferences)
- Active feedback loops (what the user just said)
- Session bridge (active tasks summary, updated every session-end)

**What does NOT belong here:**
- Client information (goes to Obsidian)
- Technical architecture decisions (Obsidian)
- Lessons learned that span projects (Obsidian)
- Anything you'd want to share with a collaborator (Obsidian)

**Lifespan:** across all sessions, but small in volume. Dozens of files, not hundreds.

### Tier 2 — Long-term memory (Obsidian vault)

**Location:** user's Obsidian vault (set up by this skill)

**What belongs here:**
- **People:** clients, partners, team — everyone you've interacted with
- **Projects:** technical decisions, stack, deploy info, architectural trade-offs
- **Rules:** how you work, language conventions, content rules
- **Lessons:** mistakes + how to avoid them next time
- **Business knowledge:** products, prices, offers, positioning

**What does NOT belong here:**
- Ephemeral tasks ("send email to John tomorrow" — that's Tier 3)
- Secrets and credentials (use a password manager)
- Raw content drafts (those live in Notion / Google Docs / wherever)

**Lifespan:** permanent. These notes should be useful 2 years from now.

### Tier 3 — Operational memory (Notion / Todoist / Linear / etc.)

**Location:** external task/content tools

**What belongs here:**
- Concrete dated tasks with owners
- Content pipeline (drafts → review → published)
- Financial records
- Team collaboration artifacts

**What does NOT belong here:**
- Knowledge that should be accessible without the tool being open
- Personal reflections, lessons, principles

**Lifespan:** weeks to months. Archive when done.

## Where does Active Tasks live?

**Active Tasks is the bridge between Tier 2 and Tier 3.**

It's in the vault (Tier 2) because Claude needs it at session start.
It references things in Tier 3 (Notion tasks) but doesn't duplicate the full task data.

Example:
```markdown
- ⚠️ Follow-up with Client X — needs decision on proposal [[Client X]] | Notion: PROJ-123
```

The one-liner + Obsidian link is in vault. The full proposal draft is in Notion.

## How Claude uses each tier

**Session start:**
1. Claude Memory auto-loads (Tier 1)
2. Claude reads `00-INDEX.md` → `Active Tasks.md` (Tier 2)
3. Only hits Tier 3 if a specific task needs it

**During work:**
- Behavioral feedback → write to Tier 1
- Knowledge / decision / person → write to Tier 2
- Action item with deadline → write to Tier 3

**Session end:**
- Claude updates Active Tasks (Tier 2)
- Any lessons go to `Lessons Learned.md` (Tier 2)
- Short session record to `Session Log.md` (Tier 2)

## The cost of not separating tiers

**All in Tier 1:** Claude Memory overflows, gets noisy, behavioral rules get ignored because they're buried under client notes.

**All in Tier 2:** Every session starts by reading megabytes of vault. Slow, expensive.

**All in Tier 3:** Claude can't see it without API calls, and task tools aren't designed for knowledge storage.

**Tiered correctly:** each system does what it's good at. Vault stays browsable. Memory stays small. Tasks stay actionable.
