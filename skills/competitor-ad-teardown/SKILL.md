---
name: competitor-ad-teardown
description: Tear down what a competitor is running on Meta (Facebook and Instagram) ads. Shows which ads they keep running (proven winners), which creatives they are scaling, what they are testing now and which landing pages get the traffic, then breaks down the top ads by hook, angle, offer and format and turns that into creative briefs for your own brand. Use for "spy on <brand>'s ads", "what is <competitor> running", "competitor ad research", or "brief me on <brand>'s winning ads".
---

# Competitor ad teardown (Meta)

Goal: learn from what a competitor keeps paying for, then brief original ads. Not to copy them.

## Data source

Skaler MCP server (`https://skaler.app/api/mcp`). Tools: `spy_on_brand`, `analyze_ad_creative`, `search_winning_ads`, `list_tracked_brands`. Brand Spy covers brands Skaler already collects; if a brand is missing, the user can add it in Skaler Brand Spy and it is collected within a few hours.

Without Skaler, the public Meta Ad Library (facebook.com/ads/library) shows a brand's active ads and start dates; read them manually and follow the same steps.

## Steps

1. **Find the brand.** Call `spy_on_brand` with the brand name, Facebook page, Ad Library URL or website domain. If the user follows brands in Skaler, `list_tracked_brands` shows them.
2. **Read the signals.** From the result, report:
   - active ads, image vs video split, and launch pace over the last 30 days (a high pace means they are testing a lot),
   - **longest running**: ads that stay live for weeks are usually profitable,
   - **most scaled**: the same creative duplicated across many ads means it is being scaled,
   - **newest**: what they are testing now,
   - the landing pages their active ads point to (offer and funnel).
3. **Break down the top ads.** Pick 3 to 5 ads across longest running and most scaled. For each, `analyze_ad_creative` with its `ad_id`. Summarise: hook (opening line and first frame), messaging angle, offer, persona, format, CTA, and how long it has run. If AI tags are missing, analyse the copy and transcript yourself.
4. **Find the pattern.** What repeats across their winners (an angle, an offer structure, a format)? What did they stop running? What are they testing that is new?
5. **Widen the view (optional).** `search_winning_ads` with the niche as `query` and `sort: "longest_running"` to check whether the pattern is category-wide or specific to this brand.
6. **Write briefs.** 3 briefs for the user's own brand. Each: the insight it comes from, angle, hook (line + first frame), format and length, offer, CTA. Make them original: same strategy, different execution.

## Output format

```
<Brand> on Meta · <date>
Active: N ads (X video / Y image) · launched last 30d: N
Proven: <ad> – running N days – hook / angle / offer
Scaling: <creative> – N copies
Testing: …
Landing pages: url (share of active ads)
Pattern: …
Briefs: 1. … 2. … 3. …
```

## Rules

- Never reproduce a competitor's footage, exact copy, brand name or claims in a brief.
- Days running and duplication are signals of performance, not proof; say so when the sample is small.
- Read-only: nothing is launched or changed.
