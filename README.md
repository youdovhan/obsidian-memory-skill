# Obsidian Memory — Claude Code Skill

Turn Obsidian into **long-term memory** for Claude Code. One afternoon of setup → Claude remembers people, projects, decisions, and lessons across every future session.

## What this skill does

When you ask Claude Code *"set up Obsidian as my memory"*, this skill:

1. Asks 5 quick questions (vault path, language, use case, MCP, hooks)
2. Creates a structured vault — folders, navigation files, templates
3. Writes a `CLAUDE.md` that wires the vault into every session
4. (Optional) installs the Obsidian MCP server
5. (Optional) sets up session-start/stop hooks
6. Gives you a next-steps checklist

Result: a working second brain that Claude reads at session start, writes to as you work, and updates at session end — without you having to re-explain anything.

## Why bother

Without structured memory:
- Every session starts from zero
- Client context is re-explained each time
- Decisions get lost
- Lessons repeat

With this skill:
- Claude knows who your clients are, their status, last interaction
- Claude knows your project stack, architectural decisions
- Claude avoids past mistakes automatically
- Session handoff is zero-effort

## Install

### Option 1 — as a global skill

```bash
# Clone into your user-level Claude skills folder
git clone https://github.com/youdovhan/obsidian-memory-skill.git \
  ~/.claude/skills/obsidian-memory
```

On Windows:
```powershell
git clone https://github.com/youdovhan/obsidian-memory-skill.git `
  "$env:USERPROFILE\.claude\skills\obsidian-memory"
```

### Option 2 — as a project skill

```bash
git clone https://github.com/youdovhan/obsidian-memory-skill.git \
  <your-project>/.claude/skills/obsidian-memory
```

### Option 3 — packaged `.skill` file

Download the latest release and import via Claude Code.

## Use

In Claude Code, just say:

> set up Obsidian as my memory

Or:

> configure my second brain with Obsidian

Claude will detect the skill and walk you through setup.

## Prerequisites

- **[Obsidian](https://obsidian.md)** installed (free)
- **Claude Code** running
- (Optional) **Node.js 18+** if you want the MCP server
- (Optional) **Git** if you want to version-control your vault (recommended)

## What gets created

A vault like this:

```
ObsidianVault/
├── 00-INDEX.md              ← Claude reads this first
├── MANIFEST.md
├── Memory/
│   ├── Active Tasks.md      ← Unfinished work
│   ├── Session Log.md       ← What happened in each session
│   ├── Lessons Learned.md   ← Errors + conclusions
│   └── Daily Reports/
├── Rules/
├── People/
│   ├── Clients MOC.md       ← CRM entry point
│   ├── Clients/
│   ├── Partners/
│   └── Team/
├── Projects/
├── Business/
└── Templates/
```

Plus a `CLAUDE.md` that tells every future session how to use it.

See [examples/example-vault-tree.md](examples/example-vault-tree.md) for what it looks like after a few months of use.

## Architecture — 3-tier memory

This skill implements a 3-tier memory model:

| Tier | System | Contents |
|------|--------|----------|
| Fast | Claude Memory (built-in) | Behavioral prefs, session bridge |
| **Long-term** | **Obsidian (this skill)** | People, projects, rules, lessons |
| Operational | Notion / Todoist / etc. | Dated tasks, content drafts |

Full reasoning in [references/architecture.md](references/architecture.md).

## Files in this skill

```
obsidian-memory-skill/
├── SKILL.md                    ← Main skill instructions (Claude reads this)
├── README.md                   ← You are here
├── templates/                  ← Files copied into user's vault
│   ├── 00-INDEX.md
│   ├── MANIFEST.md
│   ├── Active-Tasks.md
│   ├── Session-Log.md
│   ├── Lessons-Learned.md
│   ├── MOC-template.md
│   ├── Person.md
│   ├── Client.md
│   ├── Project.md
│   └── Daily-Report.md
├── references/                 ← Deeper docs, optional reads
│   ├── architecture.md         ← Why 3 tiers
│   ├── session-protocol.md     ← How Claude uses the vault
│   ├── mcp-setup.md            ← Optional MCP server
│   └── hooks-setup.md          ← Optional automation hooks
└── examples/
    ├── CLAUDE-md-example.md    ← Example project integration
    └── example-vault-tree.md   ← What a populated vault looks like
```

## FAQ

**Do I need MCP?** No. The skill works without MCP — Claude reads the vault via regular file tools. MCP is a small optimization, install later if you want.

**Do I need hooks?** No. Hooks auto-load Active Tasks at session start. Handy but not required. Try the vault manually for a few weeks first.

**What if I already have an Obsidian vault?** The skill can add its structure to your existing vault without overwriting notes. It creates the `Memory/`, `People/`, `Projects/`, etc. folders alongside what you have.

**Does this work offline?** Yes. Obsidian is local-first. Vault is just a folder of Markdown files.

**Can I sync the vault?** Yes. Use [Obsidian Sync](https://obsidian.md/sync), iCloud, Dropbox, or (recommended) git. Keep your vault in git — periodic commits protect against mistakes.

**Can I share the vault with a collaborator?** Yes, but strip PII first. See `examples/example-vault-tree.md` for format.

**Language support?** Templates are in English by default. During setup, Claude can localize them to your language.

## License

MIT — use it, fork it, ship it.

## Credits

Distilled from a real working memory system used daily to coordinate a solo AI-augmented business (content, sales, product, finance). All personal and business details stripped — what remains is the reusable pattern.
