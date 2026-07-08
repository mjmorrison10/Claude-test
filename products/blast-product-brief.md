# BLAST — Product Brief

**One-liner:** RECALL finds the moment. BLAST gets it everywhere. Multi-platform clip distribution for creators, built as the second app in the outreach engine.

**Status:** Concept — not yet built. This brief is the validation/scoping doc before any code gets written.

---

## Problem

Creators (especially in the personal-development/coaching niche RECALL already targets) cut a clip and then spend 20-40 minutes manually reformatting and re-uploading it to YouTube Shorts, TikTok, Instagram Reels, and Snapchat Spotlight — resizing, re-captioning, re-tagging, one platform at a time. That manual tax eats the time savings RECALL just gave them back.

## Target user

Same buyer as RECALL: short-form creators and coaches who already consume 10+ hours of long-form content a week and post clips across 3+ platforms. Ideal customer has already installed RECALL or is in the email-capture funnel for it — BLAST is the natural next purchase, not a new audience to find.

## How it fits the workflow

```
RECALL (find the moment) → export clip/timecode → BLAST (format + distribute) → posted everywhere
```

This is the core strategic reason to build BLAST next instead of something unrelated: it deepens the existing relationship with RECALL users rather than starting a new acquisition funnel from zero.

## MVP scope (v1)

- Upload one video/clip once.
- Auto-crop/reformat for the aspect ratios of YouTube Shorts, TikTok, Instagram Reels (9:16 covers all three — likely a single-format MVP, no per-platform re-encoding needed for v1).
- Manual caption/hashtag field per platform (auto-generation is a v2 feature, not MVP).
- Scheduled or immediate post via each platform's official API where available; manual "download + your turn to post" fallback for platforms without a usable public posting API (this determines real scope — needs a build-time spike to confirm which platforms currently allow programmatic posting for individual creators, since TikTok/Instagram API access has historically been restrictive).
- No analytics dashboard in v1 — that's a retention feature for v2, not a reason to delay launch.

## Tech approach

- Reuse the BYO-API-key, client-heavy pattern from RECALL where possible (keeps costs near zero, keeps positioning consistent: "you own your data and your keys").
- Platform posting APIs are the real unknown — unlike RECALL (single Gemini dependency), BLAST depends on 3-4 third-party platform APIs with their own auth, rate limits, and approval processes. **This is the single biggest scoping risk and should be spiked before committing to the MVP scope above.**

## Pricing (draft, needs validation)

- $29-49/mo subscription (recurring, unlike RECALL's free/lead-gen model) — this is intentionally the first recurring-revenue product in the lineup, not another free tool.
- Consider a bundle: RECALL (free) + BLAST (paid) as a package pitch once BLAST exists, rather than pricing them as fully separate products.

## Differentiation / moat

- Not competing on video editing (CapCut, etc. already own that) — competing on *distribution friction* after the edit is done.
- Owning both "find the clip" (RECALL) and "post the clip" (BLAST) creates switching cost that a single-purpose competitor can't easily replicate.

## Risks

1. **Platform API access is the real blocker, not the UI/UX.** TikTok and Instagram have historically gated posting APIs behind business verification or partner status — this needs to be confirmed before any further scoping, or BLAST becomes a "format + manual download" tool instead of true auto-posting, which is a much smaller value prop.
2. Recurring billing is new territory (RECALL has no payment infrastructure) — Stripe integration is net-new work, not a copy-paste from an existing app.

## Next steps

1. **Spike (1-2 days): confirm which platforms actually allow individual-creator programmatic posting today.** This single finding determines whether BLAST is "auto-post" or "auto-format + you post" — a fundamentally different product either way.
2. Validate willingness to pay: informal conversation with 3-5 people already in the RECALL email-capture funnel — "would you pay $29-49/mo to skip the manual re-upload step?"
3. Only after 1 and 2: write the technical MVP spec and start building.
