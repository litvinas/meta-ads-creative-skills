---
name: hook-audit
description: Audit the first 3 seconds of Meta video ads. Finds which hooks stop the scroll (hook rate) and keep people watching (hold rate), which hook types win across the account, and writes a hook test plan with new openers modelled on what works and on long-running competitor ads. Use for "audit my hooks", "why is my hook rate low", "write new hooks for my best ad", or "hook test plan".
---

# Hook audit (Meta video ads)

Hook rate = 3-second video views / impressions. Hold rate = ThruPlays (or 15-second views) / 3-second views. On Meta, around 30% hook rate is good and under 20% means the opening is failing; use the account's own numbers as the main bar.

## Data source

Skaler MCP server (`https://skaler.app/api/mcp`). Tools: `get_workspace`, `query_creatives`, `get_ai_tag_performance`, `get_ad_insights`, `search_winning_ads`, `analyze_ad_creative`. A demo account with sample data is available without connecting an ad account.

Without Skaler, ask for an Ads Manager export of video ads with impressions, 3-second video plays, ThruPlays, spend and results, and compute the rates yourself.

## Steps

1. `get_workspace`, pick the ad account.
2. **Rank video ads by hook.** `query_creatives` with `time_range: {preset: "last_14d"}`, `metrics: ["spend","hook_rate","hold_rate","ctr","cpa","roas"]`, `sort_by: "hook_rate"`, a `min_spend` that filters out ads with only a few hundred impressions. Use filters for video ads if the account mixes formats. Split the list into four groups:
   - high hook, high hold: keep and iterate,
   - high hook, low hold: the opener works, the middle loses people (fix the body, not the hook),
   - low hook, good CPA: worth a new opener, the rest of the ad already sells,
   - low hook, poor CPA: cut.
3. **Hook types that win.** `get_ai_tag_performance` with `category: "hook"` and `metrics: ["spend","hook_rate","hold_rate","roas","cpa"]`. Name the hook types (for example problem callout, curiosity, testimonial, demo-first) with the best hook rate *and* acceptable CPA. A hook that only wins clicks is a curiosity trap.
4. **Look closer at one ad.** For the best "low hook, good CPA" ad, `get_ad_insights` with its `ad_id` to read its copy and tags, so new hooks fit the existing body.
5. **Outside inspiration.** `search_winning_ads` with the product category as `query`, `format: "video"`, `sort: "longest_running"`. Long-running active ads are usually profitable. Run `analyze_ad_creative` on 2 or 3 of them and note their opening line and first frame. Borrow the pattern, never the exact words or footage.
6. **Write the test plan.** For the chosen ad, write 5 new hooks: each one line of spoken or on-screen text plus a first-frame description, labelled with its hook type and the finding it comes from. Keep the rest of the ad identical so the test only measures the hook.

## Output format

```
Account hook rate: X% (median of video ads with spend > N)
Keep/iterate: …   Fix the body: …   New opener: …   Cut: …
Winning hook types: <type> (hook X%, CPA Y) · <type> …
Test plan for <ad name>:
1. [type] "line" – first frame: … – why
…
```

## Rules

- Hooks must be true for the product. No invented stats or results.
- Avoid lines that assert something about the viewer personally ("Are you overweight?"); Meta's ad policies reject personal-attribute wording.
- Read-only. Launching the variations is a separate, explicit step (Skaler `create_launch_draft` creates paused drafts).
