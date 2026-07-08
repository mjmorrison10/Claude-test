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

## Org-chart pilot: dispatcher → specialist delegation

`sales-head-of-sales.md` + `sales-outbound-strategist.md` +
`sales-discovery-coach.md` + `sales-deal-strategist.md` — tested live with
`claude -p` against this repo as the project directory.

**Confirmed:**
- Nested delegation works. The top-level session delegated to Head of
  Sales, which in turn delegated to all three specialists — genuine
  two-level Task nesting, not simulated.
- No runaway recursion. None of the three specialists attempted to spawn
  anything further. That's the `tools:` restriction working as designed —
  they have no `Task` in their tool list, so they structurally cannot spawn
  an agent regardless of what any prompt says. That's the actual guard
  against infinite loops; prompt instructions are a secondary safeguard,
  not the mechanism.
- **Managers can't reliably see their own reports' output.** A specialist's
  result routes back to the top-level caller, not to the manager that
  delegated to it. Head of Sales never saw what its team returned, and
  correctly refused to fabricate a synthesis rather than hallucinate one —
  the top-level session ended up doing the synthesis itself from the raw
  specialist outputs.

**Adopted design, given that:** every manager agent is a **dispatcher, not
a synthesizer**. Its job is to scope the incoming request and delegate to
the right report(s) with a narrow brief — not to collect, merge, or speak
for what comes back. Synthesis happens one level up, at whoever actually
receives the results (in practice: the top-level session, i.e. the user or
the calling Claude Code instance). `sales-head-of-sales.md` is written this
way: no "synthesize your team's output" instruction, just "delegate, then
report which reports you used."

This means a deep chain of command (CEO → Head of X → team, with each layer
synthesizing before reporting up) is not what actually happens today —
what works is one dispatcher layer per delegation, with synthesis always
landing at the top. Scaling this to other divisions should follow the same
pattern: a dispatcher agent per team, tools scoped to `Task` + read-only,
no synthesis responsibility in the prompt.
