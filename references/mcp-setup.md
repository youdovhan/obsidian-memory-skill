# MCP Obsidian Setup (Optional)

MCP (Model Context Protocol) servers let Claude talk directly to Obsidian via the vault's API. It's faster than shell `cat`/`grep` for certain operations and understands Obsidian concepts (wikilinks, tags, frontmatter).

**You don't need MCP for this skill to work.** Claude can read/write the vault through plain file system access. Install MCP only if:
- You find yourself using Obsidian search often and want Claude to do the same
- You want Claude to respect wikilink resolution (`[[Client X]]` → correct file)
- You have a large vault (500+ notes) where search tools become valuable

## Option A — `obsidian-mcp` (simplest, no plugin required)

This variant reads the vault directly from the filesystem. No plugins in Obsidian needed.

### Install

```bash
claude mcp add obsidian npx -y obsidian-mcp <absolute_path_to_vault>
```

Windows example:
```bash
claude mcp add obsidian npx -y obsidian-mcp "C:\Users\you\ObsidianVault"
```

macOS / Linux example:
```bash
claude mcp add obsidian npx -y obsidian-mcp "/Users/you/ObsidianVault"
```

### Verify

```bash
claude mcp list
```

Should show:
```
obsidian    connected
```

### When it's useful
- `search_vault` — full-text search across vault, faster than shell grep for large vaults
- `read_note` / `create_note` — file ops with frontmatter parsing
- `list_available_vaults` — useful if you have multiple

## Option B — `mcp-obsidian` (via Local REST API plugin)

This variant goes through Obsidian's running instance. Obsidian must be open. Use this if you want features like graph queries or plugin integrations.

### Install

1. In Obsidian: **Settings → Community plugins → Browse** → search "Local REST API" → Install + Enable
2. In the plugin settings: click "Generate API key" → copy it
3. Add MCP server:

```bash
claude mcp add obsidian -e OBSIDIAN_API_KEY=<your_key> -- npx -y mcp-obsidian
```

4. Verify: `claude mcp list`

### Pros / cons

| | Option A (obsidian-mcp) | Option B (mcp-obsidian) |
|---|---|---|
| Obsidian running required | No | Yes |
| Plugin install | No | Yes (Local REST API) |
| Graph queries | Limited | Full |
| Speed | Fast (direct fs) | Medium (HTTP) |
| Recommendation | **Start here** | Only if you need graph |

## Troubleshooting

**"MCP server not found"** — the `npx -y` flag requires Node.js. Install Node 18+.

**"Connection refused"** (Option B) — Obsidian isn't running, or Local REST API isn't enabled, or firewall is blocking `localhost:27123` (default port).

**"Permission denied" on Windows** — wrap the vault path in double quotes, and use forward slashes or double backslashes:
```bash
claude mcp add obsidian npx -y obsidian-mcp "C:/Users/you/ObsidianVault"
```

**MCP tool fails silently mid-session** — check `claude mcp list` again. If status changed to `disconnected`, restart Claude Code.

## How to remove

```bash
claude mcp remove obsidian
```

Won't delete your vault. Just unregisters the MCP server.

## Opinion

For 90% of workflows, **Option A is enough**. Only go to Option B if you specifically need graph-view operations or plugin-level integrations.

If you're unsure — skip MCP entirely. Use plain `Read`/`Grep`/`Write` tools. Adding MCP is a small optimization, not a prerequisite.
