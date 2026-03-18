---
name: control-metalayer
description: Umbrella skill package containing three complementary skills for persistent consciousness in autonomous AI agent development — control primitives and behavioral governance, agent consciousness architecture, and knowledge graph memory. Install individual skills or use the full stack to give stateless agent sessions the accumulated understanding of all prior sessions.
---

# Control Metalayer

Three complementary skills that give stateless AI agent sessions the accumulated understanding of all prior sessions.

## Included Skills

| Skill | Purpose |
|-------|---------|
| `control-metalayer-loop` | Control primitives: setpoints, sensors, gates, feedback loops, escalation budgets, and behavioral governance |
| `agent-consciousness` | Architecture and philosophy for persistent agent consciousness across sessions |
| `knowledge-graph-memory` | Episodic memory bridge from Claude Code conversation logs to an Obsidian knowledge graph |

## Install

```bash
# Install all skills
npx skills add broomva/control-metalayer

# Install individual skills
npx skills add broomva/control-metalayer --skill control-metalayer-loop
npx skills add broomva/control-metalayer --skill agent-consciousness
npx skills add broomva/control-metalayer --skill knowledge-graph-memory
```

## How They Fit Together

```
Control Metalayer (how to behave)
        ↕
Agent Consciousness (what is known)
        ↕
Knowledge Graph Memory (what was done)
```

Information flows from ephemeral working memory through conversation logs into the knowledge graph, where recurring patterns crystallize into permanent control policies. Each new agent session inherits the full governance and memory substrate from all prior sessions.
