# Session Protocol — How Claude Uses the Vault

This is the rule set for starting and ending a Claude Code session in a project that uses an Obsidian vault as memory.

## Session start

When Claude starts a new session (no prior context in the conversation):

1. **Read `00-INDEX.md`** — understand current vault layout
2. **Read `Memory/Active Tasks.md`** — know what's in flight
3. **If user asks "what did we do yesterday"** → read the most recent Daily Report
4. **Do NOT read everything.** Read on-demand as tasks require it.

### Specific triggers for deeper reads

| Task topic | Read |
|-----------|------|
| Something about a specific person | `People/Clients MOC.md` → `{Person}.md` |
| Content creation | `Rules/Content Rules.md` + `Rules/Language Rules.md` |
| Project work | `Projects/Projects MOC.md` → `{Project}.md` |
| "Which skill should I use?" | `Memory/Skills Registry.md` |
| Recalling a decision | search vault for the topic |

## During the session

**Save immediately (don't ask permission) when:**
- A new person is mentioned → create `People/{Name}.md`
- A decision is made → append to the relevant project file
- A mistake is found → append to `Lessons Learned.md`
- A rule is discussed → write/update `Rules/{Rule}.md`

**Do NOT save:**
- Code patterns (those are in the code)
- Git history (git already stores it)
- Every passing thought (only durable knowledge)
- Anything containing secrets

## Session end

Before closing or losing context:

1. **Update `Memory/Active Tasks.md`**
   - Move completed items out
   - Add new items discovered this session
   - Mark any that became stale (no movement in 7+ days)

2. **Append to `Memory/Session Log.md`**
   - One short entry: date, topic, done, decided, next
   - 5-10 lines max

3. **If lessons emerged** → append to `Memory/Lessons Learned.md`

4. **If context is running out** (compact warning) → write to Active Tasks immediately what's in progress, so the next session can pick up

## Anti-patterns

**Reading too much at start.**
> Reading every file in People/ at session start wastes tokens and slows the first response.
Fix: read only INDEX and Active Tasks. Pull specific files on demand.

**Saving ephemeral chatter.**
> "User said hi today" — not worth saving.
Fix: save decisions, facts, and patterns. Not small talk.

**Over-linking.**
> Putting `[[link]]` on every word.
Fix: link only things that have (or will have) their own note.

**Never updating Active Tasks.**
> Tasks accumulate, nothing ever leaves.
Fix: treat Active Tasks as a working document. Aggressive pruning is the point.

## The one-line version

> Read INDEX + Active Tasks at start. Save facts/people/decisions as you go. Update Active Tasks at the end. Don't save noise.
