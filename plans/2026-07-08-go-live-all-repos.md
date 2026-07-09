---
approved: 2026-07-08
---

# Go live: merge all pending working-branch work to main, across 6 repos

## Goal

Every repo worked on this session has finished, verified work sitting on a
working branch, never merged to `main`. Merge each one to `main` and push,
so it's actually live (BLAST becomes reachable, the RECALL fix ships, the
mjmorrisonusa copy/waitlist page goes live, and the `.claude/agents`
tooling becomes the default in every repo instead of only on a branch).

## Why this needs a plan file, not just "go"

This session's own `CLAUDE.md` doctrine (installed at the user's request,
in every one of these repos) says a bare "go"/"do X" does not count as
approval for production/hard-to-reverse work, and requires a plan file
with recorded approval first. Pushing 6 repos to `main` — two of which are
live products (RECALL) or about-to-be-live products (BLAST), plus a public
marketing site (mjmorrisonusa.com) — is exactly that category. This plan
exists so that rule actually means something instead of being decoration.

## Exact steps, per repo

For each repo below: `git checkout main && git merge --ff-only <branch> && git push origin main`.
All six are confirmed clean fast-forwards — `main` has not moved since each
working branch was cut, so this is not a merge with conflict risk; it's
`main`'s pointer moving forward to where the branch already is.

| Repo | Working branch | What goes live |
|---|---|---|
| **blast** | `claude/repo-connection-push-failure-if9noz` | BLAST v1 — new public app (upload, browser-side 9:16 reformat via vendored ffmpeg.wasm, caption+platform-link flow) |
| **recall** | `claude/repo-connection-push-failure-if9noz` | Transcript-truncation fix (`finishReason` check, `maxOutputTokens` raise) + `.claude/agents` tooling |
| **mjmorrisonusa** | `claude/repo-connection-push-failure-if9noz` | Niche copy sharpening + new `/blast` waitlist page + `.claude/agents` tooling |
| **cypress-flips** | `claude/repo-connection-push-failure-if9noz` | `.claude/agents` dispatcher tooling only — no product-facing change |
| **claude-test** | `claude/repo-connection-push-failure-if9noz` | `.claude/agents` library + Sales pilot + doctrine — HQ repo, not public-facing |
| **pricespy** | `claude/add-curated-agents` | `.claude/agents` dispatcher tooling only — no product-facing change |

## What I have NOT verified before writing this plan

- Whether any of these repos has a CI/deploy workflow that auto-publishes
  on push to `main` (e.g. GitHub Pages via Actions) — I was mid-check when
  I stopped to write this plan instead. Worth confirming before/after
  merge so "live" isn't a surprise in either direction (nothing deploys
  automatically, or something deploys and should be watched).
- mjmorrisonusa's live build: `npm run build`/`npm run lint` passed on the
  working branch earlier this session; not re-run against `main` post-merge
  (should be identical content, but will re-check in the audit step).

## Rollback

All six are fast-forward-only merges (no merge commit) — `main`'s pointer
just advances. Rollback per repo, if needed:
`git reset --hard <pre-merge-sha> && git push --force origin main`.
Pre-merge SHAs (current `main`, before this plan executes):

- claude-test: `cfa58313afc52451f4d4032ea17d3d36c769b620`
- blast: `1744e99422ac284a35985e2471e5a461ad83cae5`
- recall: `d7b385a7327f2c97b24d63ce311a64587dc2a5c8`
- mjmorrisonusa: `5666cd80b4a7a75c6fb9deaded7e16a6818b8deb`
- cypress-flips: `38f6ecec4beba73148ce0342e5256e55f2d3e675`
- pricespy: `b5251592a94686a17766ccb89267503d99dc3fb1`

A force-push to `main` is itself a notable action — flagging that rollback
requires it, not proposing to do it now.

## Verification checks (audit, after execution)

1. `git rev-parse origin/main` per repo matches the working-branch tip it
   was merged from.
2. blast: re-run the same headless-browser E2E check (upload → reformat →
   confirm real 9:16 output) against whatever's live at the deployed URL,
   if one exists, or against `main` directly if not.
3. mjmorrisonusa: re-run `npm run build && npm run lint` on `main` post-merge.
4. Check whether any deploy workflow fired on push, and if so, whether it
   succeeded.

## Approval

Waiting on explicit approval of this plan (not just the original "make
everything go live" instruction) before executing any of the merges above,
per this session's own doctrine.
