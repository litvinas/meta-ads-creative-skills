# Meta ads creative skills for Claude

Three agent skills for performance marketers who run Meta (Facebook and Instagram) ads. They work in Claude Code, Claude.ai and other clients that load `SKILL.md` skills (Codex, Cursor and others that support the format).

| Skill | What it does |
|---|---|
| [weekly-creative-review](skills/weekly-creative-review/SKILL.md) | Last 7 days in one page: winners to scale, fatigued ads to cut, which hooks, angles and formats drive ROAS, and the next 3 to 5 creative tests. |
| [hook-audit](skills/hook-audit/SKILL.md) | Ranks video ads by hook rate and hold rate, finds the hook types that win for your account, and writes a 5-hook test plan for one ad. |
| [competitor-ad-teardown](skills/competitor-ad-teardown/SKILL.md) | What a competitor keeps running, scales and tests on Meta, a breakdown of their top ads, and 3 original briefs for your brand. |

All three are read-only: they analyse and write plans, they never launch, pause or edit ads.

## Data

The skills call the [Skaler](https://skaler.app) MCP server for ad data:

- Server URL: `https://skaler.app/api/mcp` (streamable HTTP, OAuth sign-in)
- Setup per client: https://skaler.app/mcp
- Without a connected ad account you get a demo account with sample data, so you can try the skills first.

Skaler is a paid product (from $83/mo billed yearly). Each skill also describes a manual fallback: an Ads Manager export or the public Meta Ad Library, so the method works without it.

## Install

**Claude Code**

```bash
git clone https://github.com/litvinas/meta-ads-creative-skills
cp -r meta-ads-creative-skills/skills/* ~/.claude/skills/
claude mcp add --transport http skaler https://skaler.app/api/mcp
```

**Claude.ai / Claude desktop:** zip a skill folder (for example `skills/hook-audit`) and upload it in Claude's Skills settings. Add Skaler as a custom connector with the URL above.

**Other clients:** copy the skill folders to wherever your client loads skills from, and add the MCP server URL.

Then ask things like "run my weekly creative review", "audit the hooks on my video ads" or "tear down Magic Spoon's Meta ads".

## Contributing

Issues and pull requests are welcome, especially improvements to the method (thresholds, test design) and fallbacks for people without Skaler.

## License

MIT
