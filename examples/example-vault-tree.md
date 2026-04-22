# Example — Populated Vault After 3 Months of Use

Shows what a working vault looks like, so the structure doesn't feel abstract.

```
ObsidianVault/
├── 00-INDEX.md                          # Entry point
├── MANIFEST.md                          # Full index
│
├── Memory/
│   ├── Active Tasks.md                  # 15-30 current items
│   ├── Session Log.md                   # Growing record, rotated monthly
│   ├── Session Log/                     # Archived months
│   │   ├── 2026-01.md
│   │   ├── 2026-02.md
│   │   └── 2026-03.md
│   ├── Lessons Learned.md               # 20-40 entries, growing slowly
│   ├── Skills Registry.md               # 10-20 skill triggers
│   └── Daily Reports/
│       ├── 2026-04-20-evening.md
│       ├── 2026-04-21-morning.md
│       ├── 2026-04-21-evening.md
│       └── Archive/
│           └── 2026-03/                 # Old reports archived monthly
│
├── Rules/
│   ├── Rules MOC.md
│   ├── Session Protocol.md
│   ├── Language Rules.md                # "EN for prompts, local lang for public content"
│   ├── Content Rules.md
│   └── Meta App Review.md               # Custom rule file
│
├── People/
│   ├── Clients MOC.md                   # Index with statuses
│   │
│   ├── Clients/
│   │   ├── Jane Doe.md                  # 🟢 Active
│   │   ├── John Smith.md                # 🔥 Hot lead
│   │   ├── Acme Corp.md                 # 🟡 Potential
│   │   └── Old Client A.md              # 🔴 Churned, kept for history
│   │
│   ├── Partners/
│   │   ├── Agency X.md
│   │   └── Consultant Y.md
│   │
│   └── Team/
│       ├── Teammate One.md
│       └── Freelancer Two.md
│
├── Projects/
│   ├── Projects MOC.md
│   ├── Dashboard.md                     # Live status of all projects
│   ├── Website Redesign.md              # 🟢 In progress
│   ├── CRM Migration.md                 # 🟢 In progress
│   ├── Newsletter Automation.md         # ⚪ Archived
│   └── Mobile App MVP.md                # 🟡 Planning
│
├── Business/
│   ├── Business MOC.md
│   ├── Pricing 2026.md
│   ├── Positioning Statement.md
│   ├── Offers.md
│   └── Competitor Notes.md
│
├── Templates/
│   ├── Person.md
│   ├── Client.md
│   ├── Project.md
│   └── Daily Report.md
│
├── Credentials/                         # Placeholders only!
│   └── Services.md                      # "Stripe key → 1Password" kind of notes
│
└── .obsidian/                           # Obsidian config (auto-created)
    ├── workspace.json
    ├── plugins/
    └── themes/
```

## What each area looks like after 3 months

### `People/Clients MOC.md`

```markdown
# Clients MOC

## 🔥 Hot (act now)
- [[John Smith]] — proposal sent, decision by Friday

## 🟢 Active
- [[Jane Doe]] — session 3 of 6, next session 2026-05-02
- [[Acme Corp]] — onboarding, contract signed 2026-04-15

## 🟡 Potential
- [[Sarah M]] — intro call 2026-04-18, sent proposal, waiting

## 🔴 Churned
- [[Old Client A]] — paused 2026-03, cited budget

## ⚪ Cold
- [[Inbound Lead B]] — replied once, no follow-up
```

### `Memory/Active Tasks.md`

```markdown
# Active Tasks

## 🔥 HOT
- [[John Smith]] — follow up on proposal decision | deadline 2026-04-25

## Sales
- [[Acme Corp]] — kickoff call, send agenda | deadline 2026-04-24

## Product
- Website redesign — finalize homepage copy with [[Jane Doe]] feedback

## Technical
- CRM migration — export from old tool, validate field mapping

## Waiting on others
- [[Sarah M]] — waiting for proposal feedback, last contact 2026-04-18

## Icebox
- Newsletter automation v2 — reconsider after Q2
```

### `Memory/Lessons Learned.md`

```markdown
# Lessons Learned

## 2026-04-10: Don't promise delivery dates without checking client calendar first
**What happened:** promised John Smith a draft for Thursday, his team was in offsite
**Why:** didn't check his availability
**Fix:** always confirm "does this week work for you" before committing

## 2026-03-22: Multi-day migration scripts need checkpoint saves
**What happened:** CRM migration crashed at record 8500/10000, had to restart
**Why:** no resume capability
**Fix:** for any data migration, write checkpoint every 500 records
```

### `Projects/Projects MOC.md`

```markdown
# Projects MOC

## 🟢 Active
- [[Website Redesign]] — homepage + about page, target launch 2026-05-10
- [[CRM Migration]] — 40% done, blocker: custom field mapping

## 🟡 Planning
- [[Mobile App MVP]] — spec phase, reviewing 3 tech stacks

## ⚪ Archived
- [[Newsletter Automation]] — v1 shipped 2026-01, monitoring, no active work
```

---

## Growth pattern

- **Month 1:** mostly empty, feels like overkill
- **Month 2:** People/ starts getting filled, Lessons has 5-10 entries
- **Month 3:** you catch yourself referring to vault in a meeting ("wait let me check what we decided")
- **Month 6:** vault is unambiguously the source of truth

The setup cost is one afternoon. The payoff compounds.
