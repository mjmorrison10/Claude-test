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
