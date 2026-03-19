---
name: knowledge-graph-memory
description: Bridge Claude Code conversation logs to an Obsidian knowledge graph for persistent agent memory. Transforms .entire/ event logs and ~/.claude/ transcripts into searchable, wikilinked markdown docs with full conversation threads, tool call details, and session metadata. Use when setting up conversation history capture, configuring knowledge graph traversal for agents, building episodic memory from session logs, or connecting agent work to an Obsidian vault. Triggers on "conversation history", "session logs to obsidian", "knowledge graph memory", "agent episodic memory", "conversation bridge", "session documentation", or "knowledge traversal".
---

# Knowledge Graph Memory

> **Broomva Stack Layer 2** (Memory & Consciousness) — part of the [24-skill Broomva Stack](https://github.com/broomva/bstack).

Bridge agent conversation logs into an Obsidian knowledge graph, giving every new session access to the full reasoning history of all prior sessions.

**Default vault**: `~/broomva-vault/` (symlinks into `~/broomva/` workspace per project).

## Quick Start

### Install the bridge script

Copy `scripts/conversation_history.py` into your project's `scripts/` directory:

```bash
cp scripts/conversation_history.py <your-repo>/scripts/conversation-history.py
chmod +x <your-repo>/scripts/conversation-history.py
```

### Generate conversation docs

```bash
python3 scripts/conversation-history.py --force          # Full regeneration
python3 scripts/conversation-history.py                   # Incremental (skip existing)
python3 scripts/conversation-history.py --dry-run         # Preview without writing
python3 scripts/conversation-history.py --limit 10        # Last 10 sessions only
```

### Wire into git hooks

Add to `.githooks/pre-push` (or equivalent):

```bash
if [ -f scripts/conversation-history.py ] && command -v python3 >/dev/null 2>&1; then
  echo "[pre-push] Updating conversation history..."
  python3 scripts/conversation-history.py 2>/dev/null && \
    git add docs/conversations/ 2>/dev/null || true
fi
```

### Add smoke check

Add to your smoke gate script:

```bash
if [ -f docs/conversations/Conversations.md ]; then
  echo "[ok] conversation history MOC present"
else
  echo "[warn] docs/conversations/Conversations.md missing"
fi
```

## What It Generates

### Conversations.md (MOC)

An index of all sessions grouped by date:

```markdown
## 2026-03-16

| Session | Branch | Turns | Duration | Topic |
|---------|--------|-------|----------|-------|
| [[session-2026-03-16-21f4eb55]] | `feature/sti-799` | 44 | 4h 15m | Implement AI Core... |
```

### Per-Session Docs

Each session doc contains:

1. **YAML frontmatter** — session_id, branch, tags, wikilinks
2. **Metadata table** — date, duration, turns, tools, attribution stats
3. **Conversation thread** — chronological timeline:
   - `> [!quote]` User prompts
   - `> [!info]` Assistant reasoning (all text blocks, not truncated)
   - `> [!example]` Tool calls (expandable, with full input details)
4. **Files touched** — all files read/written/modified
5. **Commits** — checkpoint IDs and timestamps

## Data Sources

The bridge script reads two sources:

### 1. `.entire/logs/entire.log` — Event stream

Session lifecycle events (start/end/turn), checkpoints, attribution stats, phase transitions. Requires [Entire](https://entire.dev) to be installed. If not present, script exits gracefully with code 0.

### 2. `~/.claude/projects/{KEY}/*.jsonl` — Transcripts

Full conversation transcripts from Claude Code. The project key is auto-derived from the repo path (slashes replaced with dashes). Each `.jsonl` file contains every user message, assistant response, tool invocation, and tool result.

## Noise Filtering

The script filters out internal noise from raw transcripts:

- `<task-notification>` blocks — internal task system messages
- `toolUseResult` entries — tool→assistant feedback (not real user prompts)
- `<system-reminder>` — system injections
- Messages < 5 chars — trivial acknowledgements
- Markdown headers inside callouts — converted to bold
- XML/HTML tags inside callouts — stripped

## Obsidian Rendering

All content uses Obsidian callout syntax:

- **User messages**: `> [!quote] **User** (HH:MM)`
- **Assistant reasoning**: `> [!info] **Assistant**`
- **Tool calls**: `> [!example] Tool Calls` with nested `>> [!note] **ToolName** — description` per tool
- Tool details are expanded by default (remove `-` for collapsed)

## CLAUDE.md Integration

Add to the "Context Acquisition" section:

```markdown
### Conversation History as Context

Prior sessions are indexed in `docs/conversations/`. Use them to:
- Recall prior decisions before re-solving a problem
- Understand why code looks the way it does
- Resume interrupted work on a branch
- Avoid repeating mistakes from prior sessions

Search: `grep -rl "keyword" docs/conversations/`
```

Add to "On Session Start" protocol:

```markdown
7. Scan `docs/conversations/Conversations.md` for prior sessions on current branch
```

## AGENTS.md Integration

Add to working rules:

```markdown
7. **Check conversation history for prior context** — before starting work on a branch,
   scan `docs/conversations/` for prior sessions. Use `grep -rl "keyword" docs/conversations/`
   or read `docs/conversations/Conversations.md` for a chronological index.
```

## Graceful Degradation

| Scenario | Behavior |
|----------|----------|
| No `.entire/` installed | Script exits with code 0, skip message |
| No transcripts directory | Script exits with code 0, skip message |
| Different developer machine | Transcripts dir auto-derived from repo path |
| CI (no local sessions) | Smoke warns but doesn't block |
| Pre-push without Entire | Clean skip, exit 0, `|| true` in hook |

## Lago Context Engine Integration

The knowledge graph memory now has a **server-side persistence backend** via Lago (`core/life/lago/`):

- **`lago-knowledge`** crate provides server-side frontmatter parsing, wikilink extraction, scored search, and BFS graph traversal
- **`lago-auth`** crate provides JWT auth middleware with shared-secret validation (`AUTH_SECRET`)
- **Per-user vaults**: Each authenticated user gets a Lago session (`vault:{user_id}`) for persistent `.md` storage
- **CLI**: `lago memory {status,ls,search,read,store,ingest,delete}` — ingest local vault files into Lago for remote access
- **broomva.tech dual-vault**: Chat agent tools search both server vault (`VAULT_PATH`) and user vault (`LAGO_URL`) with merged, ranked results

### Setup

```bash
# Start lagod with auth enabled
LAGO_JWT_SECRET=$AUTH_SECRET cargo run -p lagod -- --http-port 8080

# Ingest vault files
lago memory ingest ~/broomva-vault/ --token $JWT

# Search from CLI
lago memory search "consciousness" --token $JWT
```

### Environment Variables

| Variable | Where | Purpose |
|----------|-------|---------|
| `LAGO_JWT_SECRET` | lagod | Shared secret for JWT validation |
| `LAGO_URL` | broomva.tech | Lago daemon URL (e.g. `http://localhost:8080`) |
| `AUTH_SECRET` | broomva.tech | Signs JWTs for Lago auth (existing) |
| `BROOMVA_API_TOKEN` | CLI | JWT token for `lago memory` commands |

## Stack Integration

This skill is the persistence backbone for higher layers:

- **Strategy (L7)**: `braindump` files notes into the vault through this bridge
- **Strategy (L7)**: `decision-log` writes structured decisions to `vault/decisions/`
- **Strategy (L7)**: `weekly-review` scans vault changes generated by this bridge
- **Strategy (L7)**: `morning-briefing` reads action items from vault notes
- **Foundation (L1)**: `control-metalayer-loop` governance policies inform what gets persisted
- **Persistence (L0)**: Lago context engine provides server-side search, graph traversal, and per-user vault storage
