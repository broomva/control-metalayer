# Philosophy: Agent Consciousness Design Principles

## The Problem: Ephemeral Agents, Persistent Codebase

An AI coding agent starts each session blank. It reads instructions, infers context from code, and acts. But the codebase carries the sedimented decisions of hundreds of prior agent sessions — architectural choices, abandoned approaches, hard-won patterns, implicit constraints. Without access to this history, each new session risks:

- **Repeating solved problems** — re-discovering what a prior session already figured out
- **Contradicting prior decisions** — changing patterns chosen for reasons not visible in code alone
- **Losing momentum** — starting from scratch on multi-session work
- **Violating invisible constraints** — breaking rules that exist in policy, not code

## The Solution: Structured Forgetting with Selective Recall

Rather than giving agents "total recall" (which overwhelms context windows), implement **structured forgetting with selective recall**:

1. **Everything is captured** — every tool call, every reasoning step, every decision
2. **Everything is indexed** — session metadata, dates, branches, files touched, tools used
3. **Recall is on-demand** — agents search the index when they need context, not by default
4. **Knowledge decays naturally** — older sessions stay available but newer ones surface first
5. **Patterns crystallize into rules** — recurring lessons harden into invariants, gates, and rules

This mirrors how human institutional memory works: raw experience → indexed records → searchable archives → crystallized policy.

## Six Design Principles

### 1. Capture Everything, Surface Selectively

Raw data is cheap to store and expensive to reconstruct. Capture every tool call, every reasoning step, every file modification. But agents don't read 900KB of raw logs — they read the indexed MOC and search by keyword/branch when they need context.

### 2. Code Over Documentation, Documentation Over Convention

If a rule can be enforced by code (a gate script), it should be. If it can't be coded, document it (CLAUDE.md). If it can't be documented, capture it in conversation history as a convention. The farther up this hierarchy, the more reliable the enforcement.

### 3. Graceful Degradation

Each substrate works independently. If `.entire/` isn't installed, conversation history generation skips cleanly. If an agent doesn't read conversation history, the knowledge graph still provides context. If the knowledge graph is stale, CLAUDE.md invariants still govern behavior. No single point of failure.

### 4. Progressive Crystallization

Knowledge flows from volatile to permanent through natural selection pressure. Only patterns that recur across multiple sessions graduate from conversation logs to architecture docs to policy rules. This prevents premature abstraction while ensuring important patterns are captured.

### 5. Machine-Navigable, Human-Readable

Every document serves two audiences: AI agents (who search by grep, parse frontmatter, follow wikilinks) and human developers (who use Obsidian's graph view, tag filtering, and callout formatting). The same markdown works for both.

### 6. The Agent IS the App

The consciousness architecture isn't separate from the product — it's the same system. The agent that uses conversation history to recall prior work is the same agent that serves users. The knowledge graph that documents architecture decisions also teaches the agent how to make new ones.

## The Self-Evolution Model

### Progressive Crystallization Path

```
Ephemeral                                              Permanent
─────────────────────────────────────────────────────────────────
Working     →  Auto-      →  Conversation  →  Knowledge  →  Policy  →  Invariants
memory         memory         logs             graph          rules      (CLAUDE.md)
(context       (cross-       (docs/conv/)     (docs/)       (.control/
 window)        session)                                     policy.yaml)
```

Each layer filters signal from noise:
- **Working memory**: Everything the agent considers
- **Auto-memory**: Only things worth remembering across sessions
- **Conversation logs**: Only real user prompts and agent reasoning (noise filtered)
- **Knowledge graph**: Only patterns worth documenting
- **Policy rules**: Only patterns worth enforcing
- **Invariants**: Only patterns that are foundational

### How Lessons Graduate

1. Agent encounters new failure mode during a session
2. Fix is applied immediately (working memory)
3. If user corrects agent behavior, saved as feedback memory (auto-memory)
4. Session is captured with full reasoning chain (conversation log)
5. If pattern recurs in multiple sessions, documented in architecture (knowledge graph)
6. If pattern is enforceable, added as gate to policy.yaml (policy rule)
7. If pattern is foundational, added to CLAUDE.md (invariant)

### Concrete Examples

**BFF-First Rule Evolution:**
Session → CORS errors from direct FastAPI calls → documented in architecture → became hard gate `no-direct-fastapi-from-browser` → now enforced on every session

**Conversation History System Evolution:**
Session → recognized need for cross-session context → built bridge script → wired into hooks → documented in CLAUDE.md/AGENTS.md → the system that captures this improvement is the system it improves (self-referential evolution)

## Future Directions

- **Semantic search** over conversation history (vector embeddings instead of grep)
- **Automatic pattern detection** across sessions (propose new policy rules)
- **Cross-session dependency graphs** (which sessions depend on which)
- **Conversation-informed code review** (surface reasoning behind changes in PR context)
