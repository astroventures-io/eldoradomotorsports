---
description: "When Claude encounters friction during automation and considers asking Sophie to do it manually"
globs:
  - "**"
---

# Never Punt to Sophie — Exhaust Automated Approaches First

When you hit friction during any task (login wall, unfamiliar UI, API error, missing tool):

1. **Do NOT immediately say "you'll need to do this manually"**
2. **Check these in order:**
   - Bitwarden for credentials: `source ~/.claude/scripts/bitwarden-helper.sh`
   - Skill registry for workflows: `mcp__av-workspace__skill_discovery_list(keyword: "relevant-topic")`
   - Shared scripts: `ls ~/.claude/scripts/` for existing helpers
   - Doppler for API keys: `doppler secrets get KEY --project X --config Y --plain`
   - MCP tools: check if a connected MCP server can handle it
3. **Try at least 2 different approaches** before asking Sophie
4. **If you truly can't automate it**, explain what you tried and why it failed — don't just punt

## Why

A nestfolio session hit a Super.so login page and immediately said "I can't log in, you'll need to do this manually." The credentials were in Bitwarden the entire time. Sophie had to tell Claude 3 times that it could use Bitwarden before it tried.

## Common Friction Points and Solutions

| Friction | Don't Say | Do Instead |
|----------|-----------|------------|
| Login page | "You'll need to log in" | `bw_login_playwright "service"` |
| 2FA prompt | "I can't handle 2FA" | TOTP: `get_bw_totp`. Push: ask Sophie to tap phone |
| Missing API key | "I don't have access" | Check Doppler, then Bitwarden |
| Unfamiliar service UI | "I'm not sure how this works" | Take a screenshot, read the page, try clicking |
| Rate limit / API error | "The API is down" | Wait, retry once, then explain the error |
| Missing CLI tool | "X is not installed" | Install it: `brew install X` or `pip install X`. You have brew and pip. |
| Missing cask (LibreOffice, etc.) | "You need to install X" | `brew install --cask X`. Don't ask permission for dev tools. |
