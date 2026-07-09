---
approved: 2026-07-08
---

# Go live: BLAST per-platform AI captions + 4 more platforms

## Context

You used the live BLAST app and gave direct feedback: the 9:16 reformat step
is slow and its value is narrow (anyone already editing has export-to-9:16
built in already); the real value is the caption + platform-link
convenience; Facebook was missing from the platform list; and per-platform
AI-adapted captions (via Gemini) would speed things up further. All four
points were built, verified, and pushed to a branch — this plan is the
production merge, per this session's own doctrine (a bare "yes" doesn't
count as approval for shipping to a live app).

## What goes live

PR: https://github.com/mjmorrison10/blast/pull/1
Branch: `claude/blast-captions-platforms` → `main`

1. Caption/platforms panel becomes the primary "Start here" flow, reachable
   with zero upload. Reformat moves to a clearly optional section below —
   no longer gates reaching captions/platforms.
2. Platform list grows from 4 to 8: adds Facebook Reels, X, LinkedIn,
   Pinterest.
3. New "Adapt for each platform" button — one Gemini call rewrites the base
   caption per platform's conventions; each platform card gets its own
   editable result + copy button, falling back to the base caption if not
   yet adapted.
4. New Settings modal for a user-supplied Gemini API key, mirroring RECALL's
   exact pattern (BYO key, localStorage only, sent only to Google).

## Exact steps

`git checkout main && git merge --ff-only claude/blast-captions-platforms && git push origin main`.
Confirmed clean fast-forward — `main` is still at `137ea4f` (the BLAST v1
commit from the last deploy), unchanged since.

## What's verified vs. not

**Verified this session**, headless browser against the real app:
- Caption panel visible with zero upload (the core "de-emphasize reformat" change)
- Settings: key save, persists on reopen, clears correctly
- Copy-without-adapt falls back to the base caption
- Adapt-without-key errors clearly and opens Settings automatically
- Mocked Gemini response fills all 8 platform cards correctly
- Clipboard content after copy matches the *adapted* caption, not the base one
- Re-ran the original ffmpeg reformat regression test end-to-end — still
  produces a real 1080×1920 output, confirming the panel restructuring
  didn't break the optional reformat flow

**Not verified — the one open item:** the Gemini call itself was mocked
(no real API key available in this session). The request/response wiring
follows the same pattern already live and working in RECALL's transcription
call, so it's likely correct, but "Adapt for each platform" with a real key
should get a manual smoke test once this is live — that's flagged as a
test-plan checkbox on the PR itself, left unchecked on purpose.

## Rollback

Fast-forward-only merge, no merge commit — `main`'s pointer just advances.
Pre-merge SHA: `137ea4fbebfc56d270f1961b3f9fe2eff846a02c`.
Rollback if needed: `git reset --hard 137ea4fbebfc56d270f1961b3f9fe2eff846a02c && git push --force origin main`.

## Approval

Waiting on explicit approval of this plan before merging.
