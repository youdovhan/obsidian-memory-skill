---
tags: [index, navigation, claude]
aliases: [Vault Map, Index]
---

# Vault — Memory Map for Claude Code

> **This file is the entry point. Read it FIRST when starting a session.**

## Quick Navigation

| Need | Where to look |
|------|---------------|
| Full vault index | [[MANIFEST]] |
| **Active tasks** | **[[Active Tasks]]** ← unfinished work |
| **People / CRM** | **[[Clients MOC]]** |
| **Projects** | **[[Projects MOC]]** |
| **Rules (language, content, session)** | **[[Rules MOC]]** |
| Business knowledge (products, prices) | [[Business MOC]] |
| Which skill/agent to call | [[Skills Registry]] |
| Session protocol (start/end) | [[Session Protocol]] |
| Language rules | [[Language Rules]] |
| Content rules | [[Content Rules]] |
| Last session summary | `Memory/Daily Reports/` → most recent file |
| Lessons learned (avoid repeating) | [[Lessons Learned]] |

## Vault Structure

```
vault/
├── 00-INDEX.md          ← YOU ARE HERE
├── MANIFEST.md          ← Full index
├── Memory/              ← Core memory
│   ├── Active Tasks.md
│   ├── Session Log.md
│   ├── Lessons Learned.md
│   ├── Skills Registry.md
│   └── Daily Reports/
├── Rules/               ← Operating rules
│   ├── Rules MOC.md
│   ├── Session Protocol.md
│   ├── Language Rules.md
│   └── Content Rules.md
├── People/              ← CRM
│   ├── Clients MOC.md
│   ├── Clients/
│   ├── Partners/
│   └── Team/
├── Projects/            ← Technical / work projects
│   ├── Projects MOC.md
│   └── Dashboard.md
├── Business/            ← Products, prices, offers
│   └── Business MOC.md
├── Templates/           ← Note templates
└── Credentials/         ← Placeholders only (no secrets!)
```

## Memory Protocol

### When to SAVE (without asking):
- New person → `People/{Name}.md` + update relevant MOC
- New client → `People/Clients/{Name}.md` + update [[Clients MOC]]
- New partner → `People/Partners/{Name}.md`
- New team member → `People/Team/{Name}.md`
- Business decision → `Business/{Name}.md`
- New rule → `Rules/{Name}.md`
- Technical decision → `Projects/{Name}.md` (one file, not a folder)
- Error + lesson → append to [[Lessons Learned]]
- **End of each task** → update [[Active Tasks]]
- **End of session** → update [[Session Log]] + [[Active Tasks]]

### When to READ:
- **Session start** → `00-INDEX` → `Active Tasks` → last Daily Report
- Task about a client → `Clients MOC` → specific client file
- Content task → `Content Rules` + `Language Rules`
- Unclear which skill → `Skills Registry`
- Project task → `Projects MOC` → specific project

## 3-Tier Memory Architecture

| Tier | System | What lives here |
|------|--------|-----------------|
| Fast | Claude Memory (`~/.claude/projects/.../memory/`) | Behavioral corrections, bridge |
| **Long-term** | **Obsidian (this vault)** | People, rules, projects, lessons |
| Operational | Notion / Todoist / Linear | Content, tasks, finance |

---

> **Edit this file** to reflect your actual vault structure. Keep it under 150 lines — it's loaded by Claude every session.
