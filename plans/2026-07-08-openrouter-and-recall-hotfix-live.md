---
approved: 2026-07-08
---

# Go live: RECALL critical hotfix + OpenRouter provider (both apps) + BLAST video captions

## Context

Three PRs are open and reviewed-in-conversation, ready to ship:

1. **recall#1** — critical hotfix. RECALL's app body currently never executes
   for any user (async IIFE never invoked before `.catch`). Pre-existing,
   unrelated to this session's feature work, found while testing it.
2. **recall#2** — OpenRouter provider option (Gemini stays default).
3. **blast#2** — OpenRouter provider option + "suggest captions from video"
   feature (1/3/5 options per platform, vision or transcript mode).

User confirmed: "Yes, I want you to go live with all these updates." Per
this session's doctrine, that's the request — this plan is what turns it
into a reviewable, approvable, executable action.

## Exact steps

**blast** (single PR, clean fast-forward):
```
git checkout main && git merge --ff-only claude/openrouter-and-video-captions && git push origin main
```

**recall** (two PRs, ordered — hotfix first):
```
git checkout main
git merge --ff-only claude/fix-recall-init-never-runs && git push origin main
git merge --no-ff claude/openrouter-and-video-captions && git push origin main
```
The second merge is deliberately not fast-forward-only: the feature branch
was cut before the hotfix existed, so it carries its own copy of the same
one-line fix (see recall#2's PR description). Dry-run confirmed via
`git merge --no-commit --no-ff` against a simulated hotfixed main —
**auto-merges cleanly, no conflicts** (git recognizes both sides made the
identical one-line change and reconciles it, not a conflict).

## What goes live, per repo

| Repo | Before | After | Live effect |
|---|---|---|---|
| recall | `f74ee55` | hotfix + feature merged | App actually initializes again (was fully broken); OpenRouter available as a provider option |
| blast | `7f4bf82` | feature merged | OpenRouter available as a provider option; new "suggest captions from video" panel |

## Verification already performed (pre-merge, this session)

- recall hotfix: reproduced the failure in isolation, confirmed identical
  against unmodified `main`, confirmed the one-line fix resolves it (no
  `pageerror`, search box enabled, seed sources render).
- recall feature: full headless-browser pass — settings save/persist/clear
  for both providers, Gemini transcription unchanged (mocked), OpenRouter
  request shape verified, oversized-file-on-OpenRouter shows a clear error.
- blast feature: four test files — provider settings, all 8 platforms
  render, caption adapt on both providers (actual OpenRouter request shape
  verified: endpoint, Bearer auth, model, messages, response format),
  video-suggestion vision + transcript modes, count selector, AI-generated
  text HTML-escaped, vision-disabled-on-OpenRouter guard (proactive +
  reactive), large-file/video-file OpenRouter guards, and a re-run of the
  existing ffmpeg reformat regression test (still produces a real
  1080×1920 output).
- **Not verified**: live API calls with a real Gemini/OpenRouter key — no
  key was available in this session. Every provider call was tested
  against a mocked response matching the documented API shape. Flagged as
  an open item in both PRs' test-plan checklists, not silently assumed.
- This merge-sequencing dry run (above) — confirmed clean just now.

## Rollback

All fast-forward or clean auto-merges, so `main`'s pointer just advances
(recall gets one extra merge commit for the second step). Pre-merge SHAs:

- blast: `7f4bf82f1a623da93928333a3d50efd14b463491`
- recall: `f74ee55348db1e0f47a25b483660ae9f969f7a7e`

Rollback per repo if needed: `git reset --hard <pre-merge-sha> && git push --force origin main`.

## Approval

Waiting on explicit approval of this plan before executing.
