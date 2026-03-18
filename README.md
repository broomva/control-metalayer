# Agent Consciousness & Control Metalayer

> Layers 1-2 of the [Broomva Stack](https://github.com/broomva/bstack) — 24 skills across 7 layers.

Skills for persistent consciousness in autonomous AI agent development. Three complementary skills that give stateless agent sessions the accumulated understanding of all prior sessions.

## Skills

### `control-metalayer-loop` — Behavioral Governance

Control-system metalayer with setpoints, sensors, gates, feedback loops, and escalation budgets.

```bash
npx skills add broomva/control-metalayer --skill control-metalayer-loop
```

### `agent-consciousness` — Architecture & Philosophy

The synthesis: how control metalayer + knowledge graph + conversation logs form a self-evolving persistent consciousness.

```bash
npx skills add broomva/control-metalayer --skill agent-consciousness
```

### `knowledge-graph-memory` — Episodic Memory Bridge

Bridges Claude Code conversation logs (.entire/ + transcripts) to an Obsidian knowledge graph for searchable session history.

```bash
npx skills add broomva/control-metalayer --skill knowledge-graph-memory
```

## The Consciousness Stack

```
Working memory → Auto-memory → Conversation logs → Knowledge graph → Policy rules → Invariants
(ephemeral)                                                                        (permanent)
```

| Substrate | Skill | Purpose |
|-----------|-------|---------|
| Control Metalayer | `control-metalayer-loop` | How to behave (gates, policies, setpoints) |
| Knowledge Graph | `agent-consciousness` | What is known (Obsidian vault, wikilinks, MOCs) |
| Conversation Logs | `knowledge-graph-memory` | What was done (session records, tool traces) |

## Quick Start

```bash
# 1. Initialize control metalayer
python3 .agents/skills/control-metalayer-loop/scripts/control_wizard.py init . --profile autonomous

# 2. Generate conversation history
python3 scripts/conversation-history.py --force

# 3. Audit
python3 .agents/skills/control-metalayer-loop/scripts/control_wizard.py audit . --strict
```

## Self-Evolution

The system gets smarter the more it's used:

1. Agent encounters a failure mode not covered by existing policy
2. Agent fixes the immediate issue
3. Pattern is captured in conversation log (`docs/conversations/`)
4. If recurring, crystallizes into architecture doc (`docs/architecture/`)
5. If enforceable, becomes a gate in `.control/policy.yaml`
6. Future agents are governed by this rule automatically
