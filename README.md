# TIANTECH Content Agent

Weekly original TIANTECH mini-guide publishing bridge.

## Purpose

Every Thursday the agent creates one original practical self-assessment topic and a complete publication package built around:

**Kendi durumunu kontrol et, puanla ve şu üç adımı uygula.**

This workflow is not a news/RSS workflow.

## Flow

Agent -> `queue/current.json` -> GitHub Actions -> Make webhook -> LinkedIn Personal + TIANTECH Company -> archive/update

Blogger fields remain in the payload for future use, but Blogger publishing is currently disabled.

## Required GitHub Actions secret

Create this repository secret:

`MAKE_WEBHOOK_URL`

Value: the Make Custom Webhook URL.

Path:
Settings -> Secrets and variables -> Actions -> New repository secret

## Queue contract

The agent must update `queue/current.json` only when the package is complete and publishable.

Required publication state:

`"status": "READY_FOR_MAKE"`

The GitHub Action validates:
- topic_id
- topic
- personal_linkedin_text
- company_linkedin_text
- image_url
- image_filename
- image_alt_text
- at least one publish flag = true

If validation fails, the webhook is not called.

## Publishing rules

- Personal LinkedIn: Turkish expert/practitioner post.
- TIANTECH Company LinkedIn: English B2B post.
- Personal post: exactly 4 topic hashtags, no TIANTECH hashtags.
- Company post: exactly 4 hashtags; always #TIANTECH and #TIANTECHINOVASYON plus 2 topic hashtags.
- Checklist: 5-7 YES/NO controls.
- YES = 1 point.
- Exactly 3 score levels: Low / Medium / High.
- Exactly 3 improvement actions.
- The content is a practical self-assessment, not an audit, certification or compliance determination.
- Do not fabricate statistics, CVEs, laws, framework clauses, ROI or breach data.

## Visual rules

The agent generates the final publication visual. The payload must contain a downloadable HTTPS `image_url` that Make can retrieve without interactive authentication.

Preferred composition: approximately 1200x627 / 1.91:1.

Use the TIANTECH palette:
- #0047AB
- #FF8C42
- #F5F6FA
- #FFFFFF
- #0B1220

Avoid logos, text overlays, fake UI, hacker hoodies, Matrix code, giant locks/shields, sci-fi robots and generic glowing AI brains.
