---
name: control-metalayer
description: Umbrella skill package containing three complementary skills for persistent consciousness in autonomous AI agent development — control primitives and behavioral governance, agent consciousness architecture, and knowledge graph memory. Install individual skills or use the full stack to give stateless agent sessions the accumulated understanding of all prior sessions. Part of the Broomva Stack (Layers 1-2).
---

# Control Metalayer

Three complementary skills that give stateless AI agent sessions the accumulated understanding of all prior sessions.

**Stack position**: Layer 1 (Foundation) + Layer 2 (Memory & Consciousness) of the [Broomva Stack](https://github.com/broomva/bstack) — 24 skills across 7 layers.

## Included Skills

| Skill | Layer | Purpose |
|-------|-------|---------|
| `control-metalayer-loop` | Foundation (L1) | Control primitives: setpoints, sensors, gates, feedback loops, escalation budgets, and behavioral governance |
| `agent-consciousness` | Memory (L2) | Architecture and philosophy for persistent agent consciousness across sessions |
| `knowledge-graph-memory` | Memory (L2) | Episodic memory bridge from Claude Code conversation logs to an Obsidian knowledge graph |

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

## Stack Integration

Higher layers depend on this package:

| Layer | Skills | How they use control-metalayer |
|-------|--------|-------------------------------|
| Orchestration (L3) | symphony, autoany | Dispatch governed by policy gates |
| Research (L4) | deep-dive-research | Research traces persisted to knowledge graph |
| Strategy (L7) | drift-check, decision-log, weekly-review | Decisions and reviews write to vault via knowledge-graph-memory; drift-check reads control-metalayer setpoints |
