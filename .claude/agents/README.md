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

## Full rollout

Two things happened everywhere this pattern was rolled out:

1. **Every leaf specialist across all repos got a `tools:` restriction with
   no `Task` in it** — the sales pilot's three specialists already had this;
   the other ~59 installed specialist copies (engineering, security,
   testing, design, business/specialized) did not, so they got one added.
   Two files (`marketing-content-creator.md`, `specialized-pricing-analyst.md`)
   already shipped a safe `tools:` list upstream and were left alone. This
   closes the gap: no installed specialist, in any repo, can spawn an agent.
2. **A dispatcher was added per division per repo**, sized to what's
   actually installed there — no dispatcher was created for a division with
   only one specialist in a given repo (nothing to route between).

| Repo | Dispatcher(s) | Reports to each |
|---|---|---|
| PriceSpy | Head of Engineering | Backend Architect, Database Optimizer, AI Engineer, Code Reviewer, Git Workflow Master, DevOps Automator, Minimal Change Engineer, Technical Writer |
| PriceSpy | Head of Security | Application Security Engineer, Senior SecOps Engineer |
| PriceSpy | Head of Business Ops | Pricing Analyst, Retail Customer Returns, Supply Chain Strategist, Document Generator, Finance Tracker |
| PriceSpy | *(no dispatcher)* | API Tester — only tester installed, called directly |
| cypress-flips | Head of Engineering | Payments & Billing Engineer, Frontend Developer, Database Optimizer, Code Reviewer, Git Workflow Master, DevOps Automator, Minimal Change Engineer |
| cypress-flips | Head of QA | Test Automation Engineer, Accessibility Auditor |
| cypress-flips | Head of Growth | Content Creator, Analytics Reporter |
| cypress-flips | *(no dispatcher)* | UI Designer, Senior SecOps Engineer — one each, called directly |
| mjmorrisonusa | Portfolio Lead | Frontend Developer, UI Designer, Code Reviewer, Git Workflow Master, Minimal Change Engineer, Accessibility Auditor (whole 6-agent set — too small to split into divisions) |
| recall | Recall Lead | Frontend Developer, UI Designer, Code Reviewer, Git Workflow Master, Minimal Change Engineer (whole 5-agent set) |

Each dispatcher lives only in the repo it manages (custom subagents are
project-scoped — a dispatcher can't reach across repos). This HQ copy keeps
just the Sales pilot as the reference example of the pattern; the real,
in-use dispatchers are in each project's own `.claude/agents/`.

**Still open:** only the Sales pilot has actually been live-tested with
`claude -p`. The others follow the same, now-confirmed mechanics (dispatcher
with `Task`, specialists without it), but haven't each been individually
run. If one behaves unexpectedly, treat it the same way the Sales pilot's
synthesis bug was handled — check what actually happened before trusting
what the agent claims happened.
