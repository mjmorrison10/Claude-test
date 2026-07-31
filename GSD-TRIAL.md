# GSD Core trial (installed 2026-07-31)

GSD Core 1.8.0 is installed **locally in this repo only** — deliberately not
global, and not yet in recall / blast / pulse / Hooklabs / mjmorrisonusa. This
is a trial in the HQ repo to see whether the loop earns its overhead before it
touches anything on the revenue path.

Install command used:

```
npx @opengsd/gsd-core@latest --claude --local --profile=standard
```

## Why `--profile=standard`

The default `full` profile costs roughly **12k tokens of cold-start context**.
`standard` is 15 skills at about **700**. This repo already loads a CLAUDE.md,
15 ECC rule files and 28 agents, and `.claude/settings.json` declares the ECC
plugin (`affaan-m/ECC`), which pulls in a large skill set of its own on machines
where it resolves. Adding 12k tokens on top of that is the opposite of what a
context-engineering tool is for.

Widen later with `gsd update` if `standard` turns out too thin.

## What it added

- 23 `/gsd-*` commands in `.claude/commands/`
- 8 `gsd-*` agents in `.claude/agents/` (the existing 28 are untouched)
- `.claude/gsd-core/` runtime, `.claude/hooks/`, `.claude/scripts/`
- 585 files, ~9 MB, all additive — no existing file was modified or deleted

## Known gap: it does not enforce our approval gate

GSD's loop is **Discuss → Plan → Execute → Verify → Ship**. Our doctrine is
**Plan → Approve → Execute → Audit**, and the approval gate is the hard rule:

> No execution until the user has explicitly reviewed and approved the plan
> *file*. "Go", "resume", or "do X" is not approval of an unreviewed plan.

GSD's `Discuss` runs *before* planning, not after, so nothing in its loop stops
an agent going Plan → Execute unattended. **Do not treat `/gsd-execute-phase` as
satisfying the gate.** Until this is settled, the gate stays a human step:
read the plan file, approve it explicitly, record `approved: YYYY-MM-DD` in the
frontmatter, and only then execute.

Worth checking during the trial whether `hooks.workflow_guard` or the commit
validation hook (both opt-in, currently off) can enforce the gate directly. If
one can, that is the piece that makes the two systems actually compose.

## Hooks are not shared

The installer wrote 15 hook commands into `.claude/settings.local.json`, 11 of
which hardcode an absolute path to the node binary of the machine that ran the
install. That file is **gitignored** — it would break on any other machine. Run
the install command above on each machine to regenerate it.

Consequence: cloning this repo gives you the commands and agents (plain
markdown, they work anywhere) but no hooks until you run the installer locally.
