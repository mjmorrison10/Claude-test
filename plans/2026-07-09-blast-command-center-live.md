---
approved: 2026-07-09
---

# Go-live: BLAST posting command center (phase 1)

## Context

Every other branch across all 5 connected repos (claude-test, blast, recall,
mjmorrisonusa, cypress-flips) is already merged to `main` as of this session's
prior go-lives. The one remaining unmerged item is BLAST's
`claude/posting-command-center` branch — phase 1 of the posting-command-center
work (per-platform status tracking, session persistence, combined
copy+open+mark action), built and tested per
`blast/plans/2026-07-08-posting-command-center.md` (already approved, PR #3).
This plan is the separate go-live approval to merge that PR to `main`, per
the doctrine — a bare "push it live" instruction doesn't substitute for this
step.

## Repo / branch state (verified by direct `git log` diff against `origin/main`)

| Repo | Branch | Status |
|---|---|---|
| claude-test | `claude/repo-connection-push-failure-if9noz` | already merged, 0 commits ahead |
| blast | `claude/blast-captions-platforms` | already merged, 0 commits ahead |
| blast | `claude/openrouter-and-video-captions` | already merged, 0 commits ahead |
| blast | **`claude/posting-command-center`** | **1 commit ahead — `d33b16e`, not yet merged** |
| recall | `claude/fix-recall-init-never-runs` | already merged, 0 commits ahead |
| recall | `claude/openrouter-and-video-captions` | already merged, 0 commits ahead |
| mjmorrisonusa | `claude/repo-connection-push-failure-if9noz` | already merged, 0 commits ahead |
| cypress-flips | `claude/repo-connection-push-failure-if9noz` | already merged, 0 commits ahead |

Only blast needs a merge action for this go-live.

## What goes live

Single commit `d33b16e` — "Add posting command center: per-platform status +
session persistence" — merging `claude/posting-command-center` into
`blast`'s `main` (currently at `13560cb`, fast-forward candidate, no
conflicts).

Ships: per-platform posting status (not started → copied → opened → posted,
or skipped) with a status chip and left-border color per card; a session bar
showing "N of 8 posted" with a progress fill; a combined copy+open button
that copies the caption, opens the platform's upload page, and advances
status in one click; session persistence to `localStorage`
(`blast_session_v1`) surviving refresh; a "Reset session" control. Fully
additive — an empty/absent session key falls back to the prior empty-start
behavior, so this can't regress anyone currently using BLAST without a saved
session.

## Verification already performed

26-check headless-Chromium suite (`test-command-center.mjs`), all passing:
session bar + empty state, 8 status chips/combined buttons/mark controls,
copy+open flow (copies clipboard, opens URL, advances to "opened", doesn't
falsely count as posted), mark-posted (count increments, card class),
status non-regression (copy after posted doesn't demote it), skip, post-URL
paste auto-marks posted, full persistence across reload (caption, statuses,
post URL, summary), reset clears everything.

Regression suites re-run green on the same branch: provider/settings (18
checks), video-suggest (12 checks), transcript+OpenRouter (8 checks) — no
prior feature broken by this addition.

Not yet verified (unchanged from every prior BLAST/RECALL go-live this
session): a live Gemini/OpenRouter API call with a real user key. All
provider-call correctness has been verified via mocked network responses
only.

## Merge steps

```
cd /workspace/blast
git fetch origin main claude/posting-command-center
git checkout main
git merge --ff-only claude/posting-command-center
git push -u origin main
```

Fast-forward only — if `origin/main` has moved since the SHA recorded below,
stop and re-verify before forcing anything.

## Rollback

Pre-merge `main` SHA: `13560cbbe82fd3b7b2528425d050bb5f18abdb64`.
`git revert d33b16e` or `git reset --hard 13560cbb` + force-push (only with
explicit approval) restores prior state. GitHub Pages redeploys
automatically off `main`.

## Doctrine

Per the standing plan-approve-execute-audit doctrine adopted this session:
this plan requires explicit approval (recorded as `approved: YYYY-MM-DD` in
the frontmatter above) before the merge step runs. The user's "push them all
live" instruction is the trigger to prepare this plan, not the approval
itself.
