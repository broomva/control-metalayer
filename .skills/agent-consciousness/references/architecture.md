# Architecture: Agent Consciousness System

## Data Flow

```
┌─────────────────────────────────────────────────────────────┐
│ Claude Code Session (real-time agent execution)             │
└──────────┬──────────────────────────────────────────────────┘
           │ events (JSON lines)
┌──────────▼──────────────────────┐
│ .entire/logs/entire.log          │ Session lifecycle, checkpoints,
│                                  │ attribution, phase transitions
└──────────┬──────────────────────┘
           │ transcripts (JSONL per session)
┌──────────▼──────────────────────┐
│ ~/.claude/projects/{KEY}/        │ Every message, tool call,
│ {session-uuid}.jsonl             │ tool result, git branch, version
└──────────┬──────────────────────┘
           │ bridge script
┌──────────▼──────────────────────┐
│ scripts/conversation-history.py  │ Parses both sources, merges,
│                                  │ filters noise, builds timeline,
│                                  │ generates Obsidian markdown
└──────────┬──────────────────────┘
           │ session docs
┌──────────▼──────────────────────┐
│ docs/conversations/              │ Per-session records with:
│ ├── Conversations.md (MOC)       │ conversation thread, tool calls,
│ └── session-{date}-{id}.md       │ files touched, commits, wikilinks
└──────────┬──────────────────────┘
           │ wikilinks + tags
┌──────────▼──────────────────────┐
│ Knowledge Graph (docs/)          │ Architecture, features, operations,
│                                  │ security, workflows, control docs
└──────────┬──────────────────────┘
           │ patterns crystallize
┌──────────▼──────────────────────┐
│ Control Metalayer (.control/)    │ policy.yaml, state.json,
│                                  │ topology.yaml, commands.yaml
└──────────────────────────────────┘
```

## Control Metalayer Components

| Component | File | Purpose |
|-----------|------|---------|
| Setpoints | `METALAYER.md`, `evals/control-metrics.yaml` | 15 target metrics |
| Sensors | CI, test suites, linters, PR reviews | Measure current state |
| Controller | `.control/policy.yaml` | Hard gates (block) + soft gates (warn) |
| Actuators | Code edits, doc updates, policy changes | Close the gap |
| State | `.control/state.json` | Current metrics, gate results |
| Topology | `.control/topology.yaml` | Code ownership zones |
| Commands | `.control/commands.yaml` | Available operations |

## Gate Sequence

```
smoke (1-2 min)  →  check (2-3 min)  →  test (5-10 min)  →  push  →  review  →  resolve
   │                    │                    │                  │         │          │
   env sanity          lint                 unit tests        CI        agents      reply
   typecheck           format              integration       E2E       triage      resolve
   ruff                prettier            audit                       fix P1
   harness valid
```

## Hard vs Soft Gates

**Hard gates block merge:**
- `no-direct-fastapi-from-browser` — BFF proxy pattern
- `tenant-isolation-required` — schema-based multi-tenancy
- `no-m2m-tokens-in-browser` — session tokens only
- `pr-review-comments-resolved` — all P1 comments fixed

**Soft gates warn:**
- `unified-state-only` — prefer single state slice
- `reusable-components-first` — check shared components
- `doc-update-on-tool-change` — keep docs current

## Knowledge Graph Structure

```
/                                ← Obsidian vault root
├── CLAUDE.md                    ← Invariants + agent protocol
├── AGENTS.md                    ← Tool catalog + working rules
├── METALAYER.md                 ← Control loop definition
├── PLANS.md                     ← Active execution plans
├── docs/
│   ├── Documentation Hub.md     ← MOC of MOCs
│   ├── architecture/            ← System design
│   ├── conversations/           ← Session history (auto-generated)
│   ├── agentic-harness/         ← Execution framework
│   ├── control/                 ← Metalayer documentation
│   ├── workflows/               ← Procurement/domain workflows
│   └── {section}/               ← Features, ops, security, dev
├── .control/                    ← Machine-readable control state
│   ├── policy.yaml
│   ├── state.json
│   ├── topology.yaml
│   └── commands.yaml
└── .entire/                     ← Session event logs
    └── logs/entire.log
```

## Frontmatter Schema

Every doc carries structured YAML for machine navigation:

```yaml
---
title: Document Title
description: One-line description
tags:
  - stimulus/{section}
  - {topic-tag}
type: architecture | guide | specification | moc | conversation
status: active | deprecated | draft
created: YYYY-MM-DD
updated: YYYY-MM-DD
related:
  - "[[Connected Doc]]"
---
```

## Session Doc Structure

Each conversation session generates a doc with:

```yaml
---
title: "First user message (truncated)..."
type: conversation
session_id: uuid
branch: feature/branch-name
tags:
  - stimulus/conversations
  - branch/{branch-name}
related:
  - "[[Conversations]]"
  - "[[CLAUDE]]"
---
```

Content sections:
1. Metadata table (session ID, date, duration, turns, tools, branch)
2. Conversation thread (chronological user → assistant → tools timeline)
3. Tool call details (nested expandable callouts per tool)
4. Files touched
5. Commits

## Hooks Chain

| Hook | Trigger | Actions |
|------|---------|---------|
| pre-commit | Before staging | `make smoke` (fast static checks) |
| pre-push | Before push | `make check`, conversation history update, stage docs |
| CI | Push/PR | `make ci` (smoke + check + test) |
| Nightly | Cron | `make control-audit-strict` |

## Retry Budget and Escalation

Agents get 2 attempts to fix a failing gate. On third failure, escalate to human. This prevents infinite correction loops while allowing autonomous recovery from transient issues.
