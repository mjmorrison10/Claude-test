# Outreach Engine — Product Roadmap

Business HQ tracking doc for the pivot from video-editing/clipping to AI-powered web apps for the male self-improvement/creator niche. Michael Morrison, positioning as a growth expert, not just an editor.

## Live

### RECALL — clip-memory search tool
- **Status:** Shipped, live, free (lead-gen tool).
- **What it does:** Turns podcast/interview transcripts into a searchable memory layer. Paste a transcript or upload audio (Gemini-powered transcription) — search any phrase, get every moment it appears across every source, timecoded.
- **Repo:** `mjmorrison10/recall`
- **Landing page:** `mjmorrisonusa.com/#/recall`
- **Recent fix:** Truncation bug — long transcripts (>~40min) were silently cut off because the Gemini API response wasn't checked for `finishReason`. Fixed by adding a `MAX_TOKENS` check and raising `maxOutputTokens` to 16000. See `recall` repo commit `3ff774f`.

## In development

### BLAST — multi-platform clip distribution
- **Status:** Concept + waitlist page live. Not yet built.
- **What it does:** Takes the clip RECALL just found and formats/distributes it across YouTube Shorts, TikTok, Instagram Reels, and Snapchat Spotlight from one upload.
- **Why it's next:** Deepens the existing RECALL relationship instead of starting a new acquisition funnel — `RECALL (find) → BLAST (distribute)` is one workflow, not two separate sales.
- **Full brief:** `products/blast-product-brief.md` (this repo)
- **Waitlist page:** `mjmorrisonusa.com/#/blast`
- **Open risk:** Platform posting-API access for individual creators (TikTok/Instagram have historically gated this) needs a spike before MVP scope is locked.
- **Pricing model:** First recurring-revenue product in the lineup ($29-49/mo), unlike RECALL's free/lead-gen model.

## Positioning notes

- Site copy (mjmorrisonusa.com) updated to name the niche explicitly — "coaches, podcasters, and creators in the personal-development space" — instead of generic "creators and small businesses." Applies to Home, WebDev, and RECALL landing pages.
- Brand cleanup (removing Andrew Tate / Real World associations) already completed prior to this roadmap — see `BRAND-CLEANUP-PLAN.md` in the `mjmorrisonusa` repo. Current positioning reframes the same growth story (314K followers, 50M+ monthly views, 9+ platforms) without naming the org or individuals involved.

## Sequencing logic

1. RECALL exists and works → fix the truncation bug so it's trustworthy for long-form content (done).
2. Sharpen who it's for → niche-targeting copy pass so the right buyers self-select (done).
3. Give RECALL users a second reason to stay in the ecosystem → BLAST brief + waitlist (done — build not started).
4. Validate before building → confirm platform API access and willingness to pay before writing BLAST's MVP spec (not started).
