# Agent Library (HQ)

Full curated set of Claude Code subagents pulled from
[mjmorrison10/agency-agents](https://github.com/mjmorrison10/agency-agents)
(288 agents total), narrowed down to the 23 relevant to this account's actual
projects: PriceSpy, cypress-flips, mjmorrisonusa, and recall.

This repo holds the canonical/reference copy of every agent in the set. Each
project repo below has its own `.claude/agents/` with just the subset that
applies to it, so agents are active without needing this repo attached too.

| Repo | Stack | Agent count |
|---|---|---|
| [PriceSpy](https://github.com/mjmorrison10/pricespy) | Flask, eBay API, Firebase, Gemini AI | 16 |
| [cypress-flips](https://github.com/mjmorrison10/cypress-flips) | Static storefront, Stripe, Firebase | 13 |
| [mjmorrisonusa](https://github.com/mjmorrison10/mjmorrisonusa) | React/Vite/Tailwind portfolio | 6 |
| [recall](https://github.com/mjmorrison10/recall) | Vanilla JS PWA | 5 |

`blast` was skipped — no content yet to judge relevance against.

To add an agent to a project, copy its `.md` file from here into that repo's
`.claude/agents/`.

## Org-chart pilot: manager → specialist delegation

`sales-head-of-sales.md` + `sales-outbound-strategist.md` +
`sales-discovery-coach.md` + `sales-deal-strategist.md` are a test of
hierarchical delegation (you → Head of Sales → specialists → results flow
back up), inspired by a two-tier chain-of-command setup.

How it's built:
- **Head of Sales** (`tools: Task, Read, Write, TodoWrite`) is the only one
  of the four with a delegation tool, and its prompt explicitly limits it to
  delegating to its three named reports only, one call each, no re-chaining.
- **Outbound Strategist / Discovery Coach / Deal Strategist**
  (`tools: Read, Write, Grep, Glob, WebSearch, WebFetch`) have no `Task` in
  their tool list at all — they structurally cannot spawn any agent,
  regardless of what a prompt tells them to do. This is what actually
  prevents runaway recursion; the prompt instructions are a secondary
  safeguard, not the mechanism itself.

**Known limitation:** whether a subagent can itself call `Task` to invoke a
further subagent (true nested delegation, not just you → one subagent) is a
Claude Code platform capability, not something these files control. It was
not testable from the remote multi-repo session that built this — that
session's own subagent tool only exposes a fixed built-in set and does not
load custom `.claude/agents/*.md` from any project directory. To find out if
it actually works, open Claude Code (CLI or IDE) with this repo as the
project directory and ask it to delegate a sales task to Head of Sales —
watch whether it in turn spawns the three specialists or just does the work
itself. If nesting isn't supported, flatten this to one dispatcher agent
that delegates directly to all specialists instead of routing through a
middle manager.
