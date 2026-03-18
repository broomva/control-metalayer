---
name: agent-consciousness
description: Persistent consciousness architecture for autonomous AI agent development. Synthesizes three substrates — control metalayer (behavioral governance), Obsidian knowledge graph (declarative memory), and conversation log bridge (episodic memory) — into a self-evolving context layer. Use when designing agent memory systems, implementing cross-session context persistence, building knowledge graphs for AI agents, setting up conversation history capture, or architecting feedback loops that let agents learn from prior sessions. Triggers on "agent memory", "session persistence", "knowledge graph", "conversation history", "consciousness stream", "cross-session context", "agent learning", or "self-evolving development".
---

# Agent Consciousness Architecture

> **Broomva Stack Layer 2** (Memory & Consciousness) — part of the [24-skill Broomva Stack](https://github.com/broomva/bstack).

Implement a persistent consciousness layer for AI coding agents that gives every new stateless session the accumulated understanding of all prior sessions.

## Core Concept

Each agent session is ephemeral — it starts blank. The consciousness architecture weaves three systems into a single persistent substrate:

1. **Control Metalayer** — How to behave (gates, policies, setpoints, feedback loops)
2. **Knowledge Graph** — What is known (Obsidian vault, wikilinks, MOC navigation, tag taxonomy)
3. **Conversation Logs** — What was done (session records, tool traces, decision chains)

See `references/architecture.md` for the complete system design and data flow.
See `references/philosophy.md` for design principles and the self-evolution model.

## Quick Start

### New repo (from scratch)

1. Initialize control metalayer with `control-metalayer-loop` skill
2. Create `docs/` with Obsidian vault structure (MOC pattern per section)
3. Install conversation history bridge with `knowledge-graph-memory` skill
4. Wire hooks: pre-push regenerates conversation docs, smoke validates MOC

### Existing repo with control metalayer

1. Add `docs/conversations/` directory
2. Install `scripts/conversation-history.py` from `knowledge-graph-memory` skill
3. Update CLAUDE.md context acquisition to reference conversation history
4. Update AGENTS.md working rules to check prior sessions
5. Add pre-push hook entry for incremental conversation doc generation

## The Three Substrates

### Control Metalayer (How to Behave)

Closed-loop feedback: `Setpoints → Sensors → Controller → Actuators → Verify → loop`

- **Setpoints**: Quality targets (pass_at_1 ≥ 0.70, gate_pass_rate ≥ 0.85)
- **Sensors**: CI, tests, linters, PR review agents, harness validation
- **Controller**: `.control/policy.yaml` — hard gates (block) + soft gates (warn)
- **Actuators**: Code edits, doc updates, policy changes
- **Gate sequence**: `smoke → check → test → push → review → resolve`

### Knowledge Graph (What Is Known)

An Obsidian vault with wikilinks, tag taxonomy, and MOC navigation:

```
docs/
├── Documentation Hub.md    ← MOC of MOCs (start here)
├── architecture/           ← System design
├── conversations/          ← Session history (auto-generated)
├── agentic-harness/        ← Execution framework
├── control/                ← Metalayer docs
└── {section}/              ← Features, operations, security, etc.
```

Every doc has YAML frontmatter with `tags:`, `related:`, `type:` for machine navigation.

### Conversation Logs (What Was Done)

Raw session data bridged to Obsidian:

```
.entire/logs/entire.log  ──┐
                            ├──▶ conversation-history.py ──▶ docs/conversations/*.md
~/.claude/projects/*.jsonl ─┘
```

Each session doc: full conversation thread, tool call details (expandable callouts), files touched, commits, branch metadata, wikilinks to knowledge graph.

## The Consciousness Stack

From most ephemeral to most permanent:

| Layer | Lifetime | Location | Update Frequency |
|-------|----------|----------|------------------|
| Working memory | Single session | Context window | Every message |
| Auto-memory | Cross-session | `~/.claude/.../memory/` | On learning events |
| Conversation logs | Permanent | `docs/conversations/` | Pre-push hook |
| Knowledge graph | Permanent | `docs/` | On architectural changes |
| Policy rules | Permanent | `.control/policy.yaml` | On new failure modes |
| Invariants | Permanent | `CLAUDE.md` | Rarely (foundational) |

Information flows upward: working observations → memory notes → session records → architecture docs → enforced rules → core invariants. Only recurring patterns crystallize into permanent rules.

## Self-Evolution Cycle

```
Agent Session → Conversation Log → Knowledge Graph → Control Metalayer → Governs Next Session
```

1. Agent encounters failure mode not covered by existing policy
2. Agent fixes immediate issue
3. Pattern captured in conversation log
4. If recurring, crystallizes into architecture doc
5. If enforceable, becomes a gate in `.control/policy.yaml`
6. Future agents governed by this rule automatically

## Agent Session Protocol

### On Session Start

1. Read CLAUDE.md (invariants), AGENTS.md (tools), METALAYER.md (control loop)
2. Check PLANS.md (active plan to continue?)
3. Check `.control/state.json` (current metrics)
4. Check `git status + git log` (recent changes)
5. Scan `docs/conversations/Conversations.md` for prior sessions on current branch

### Before Making Changes

Search conversation history: `grep -rl "keyword" docs/conversations/`
Traverse knowledge graph via MOC files and wikilinks.
Check if prior sessions already solved this problem.

### On Task Completion

1. Run `make smoke` (validate gates)
2. Update docs per Doc-Update-on-Push policy
3. Pre-push hook auto-regenerates conversation history

## Stack Integration

This skill is consumed by higher layers:

- **Strategy (L7)**: `decision-log` and `weekly-review` persist outputs through the consciousness substrate
- **Strategy (L7)**: `drift-check` reads control-metalayer setpoints to detect misalignment
- **Strategy (L7)**: `braindump` and `morning-briefing` read/write vault via knowledge-graph-memory
- **Orchestration (L3)**: `symphony` and `autoany` inherit session context through the consciousness stack
