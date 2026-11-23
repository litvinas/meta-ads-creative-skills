---
name: weekly-creative-review
description: Weekly Meta ads creative review for a media buyer or creative strategist. Pulls the last 7 days of ad performance, separates real winners from noise, flags fatigue, finds which hooks, angles and formats drive ROAS, and ends with a short list of next creative tests. Use for "weekly creative review", "what's working in my Meta ads", "which ads should I scale or kill", or a Monday ads report.
---

# Weekly creative review (Meta ads)

Goal: one page a media buyer can act on in five minutes. What won, what is dying, why, and what to test next.

## Data source

This skill expects the Skaler MCP server (`https://skaler.app/api/mcp`, OAuth). Tools used: `get_workspace`, `get_account_summary`, `query_creatives`, `get_ai_tag_performance`, `get_breakdowns`.
Without a connected ad account, Skaler serves a demo account with sample data, which is fine for trying the skill.

If the Skaler tools are not available, ask the user for an Ads Manager export (ad name, spend, purchases or results, revenue, impressions, 3-second video views, ThruPlays, CTR, CPM) for the last 7 days and the 7 days before, and run the same steps on that file.

## Steps

1. **Pick the account.** Call `get_workspace`. If there is more than one ad account, ask which one. Note the currency.
2. **Headline numbers.** `get_account_summary` with `time_range: {preset: "last_7d"}`. Report spend, ROAS, CPA, purchases and the % change versus the previous 7 days. Say plainly if spend moved more than results.
3. **Winners.** `query_creatives` for the last 7 days, `metrics: ["spend","roas","cpa","ctr","hook_rate","hold_rate","purchases"]`, `sort_by: "roas"`, `min_spend` set to roughly 2x the account's target CPA (ask for the target if unknown; otherwise use 2x the account CPA). A winner needs both: ROAS at or above the account average and enough spend to mean something. List at most 5.
4. **Losers and fatigue.** Same call sorted by `cpa` descending (`ascending: false`) with the same `min_spend`. Flag ads that:
   - spent the most but sit below account ROAS,
   - have a hook rate under 20% (video), or
   - dropped sharply versus the previous week (re-run `query_creatives` with `time_range: {preset: "last_14d"}` or explicit dates for the prior week and compare).
   Fatigue usually shows as CPM and CPA rising while CTR falls on an ad that used to win.
5. **Why it works.** Call `get_ai_tag_performance` for `category: "hook"`, then `"messaging_angle"`, then `"visual_format"` over the last 14 days (more data, steadier ranking). Name the top 2 values in each by ROAS with their spend share, and any value that takes a big share of spend at below-average ROAS.
6. **Where it works (optional).** If one winner carries most of the spend, `get_breakdowns` with `ad_name` set to it and `breakdown_types` for placement and age/gender, to see who buys.
7. **Next tests.** Propose 3 to 5 tests, each tied to a finding above:
   - iterate a winner (same angle, new hook or new first frame),
   - give a winning angle a format it has not been tried in,
   - replace a fatigued ad with a fresh hook on the same body.
   Keep each test to one change so the result is readable.

## Output format

```
Week of <dates> · <account> · <currency>
Spend X (±%), ROAS X (±%), CPA X (±%), purchases X (±%)

Scale: <ad> – ROAS, CPA, spend, why (tag/angle)
Watch: <ad> – what is slipping
Cut: <ad> – spend wasted, reason

What wins: hooks <a, b> · angles <a, b> · formats <a, b>
Next tests: 1. … 2. … 3. …
```

## Rules

- Never call a winner on tiny spend; say "not enough data" instead.
- Use the account's own averages as the bar, not generic industry benchmarks.
- Read-only: this skill never launches, pauses or edits ads. If the user wants to act on a test, they can use Skaler's launch draft tools (`create_launch_draft`, paused by default) in a separate step.
