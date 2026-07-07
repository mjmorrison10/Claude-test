# Business HQ

This repository is the operational headquarters for a **growth agency serving male self-improvement creators** (coaches, course sellers, podcasters in the Tate / TRW / "become a better man" space).

**Core idea:** short-form clip editing is the cheap, easy *foot in the door*. The money is in the **upsell ladder** — content management, websites, and AI automation.

Everything here is committed to git, so the business's brain, track record, and playbooks can never be "banned" or lost again.

## Map

| Folder | What lives here |
|---|---|
| [`business/`](business/) | Strategy, ideal customer, offers & pricing — the *why* and *what* |
| [`playbooks/`](playbooks/) | Step-by-step procedures Claude follows — the *how* |
| [`crm/`](crm/) | The lead pipeline. Who we're talking to and where they stand |
| [`outreach/`](outreach/) | Cold + warm message templates |
| [`portfolio/`](portfolio/) | Proof. Every clip we make gets logged here so the track record rebuilds itself |

## Current status

- **Stage:** Setup / cold-start. Rebuilding proof after old social accounts were lost.
- **Warm asset:** *Come On Man Podcast* — former client, good terms. Highest-priority reactivation. See [`crm/pipeline.md`](crm/pipeline.md).
- **First engine to run:** the Free-Sample Outreach engine. See [`playbooks/free-sample-outreach.md`](playbooks/free-sample-outreach.md).

## How to use this with Claude

Tell Claude things like:
- *"Reactivate Come On Man — draft the re-engagement message and ask for a testimonial."*
- *"Find me 10 coaches that fit our ICP and add them to the pipeline."*
- *"Clip episode X into three shorts and log them in the portfolio."*
- *"What's in my pipeline right now and what's the next action on each?"*

Claude reads these files for context, does the work with the connected tools, and updates the files so the HQ always reflects reality.
