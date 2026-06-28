# control-metalayer (DEPRECATED — consolidated into the broomva/skills monorepo)

> **Status:** 6-month deprecation window (until 2026-12-27). The control-metalayer
> sub-skills have been **consolidated** into a single skill, `agentic-control-kernel`,
> in the [broomva/skills](https://github.com/broomva/skills) monorepo (BRO-1561 / BRO-1570).

## New install

```bash
npx skills add broomva/skills --skill agentic-control-kernel
```

`agentic-control-kernel` is the unified successor of the three skills this repo used to
ship separately. Their surfaces were merged (control primitives + episodic memory +
consciousness stack) into one kernel rather than three overlapping installs:

| Former standalone sub-skill | Now part of |
|---|---|
| `control-metalayer-loop` (setpoints, sensors, shields, policy gates) | `agentic-control-kernel` |
| `agent-consciousness` (governance + knowledge-graph + episodic-memory architecture) | `agentic-control-kernel` |
| `knowledge-graph-memory` (conversation-log → Obsidian bridge) | `agentic-control-kernel` |

The old `npx skills add broomva/control-metalayer` continues to resolve through the
deprecation window, but new installs should use the command above (the monorepo layout
also fixes the remote root-install script-drop hazard, vercel-labs/skills#1523).
