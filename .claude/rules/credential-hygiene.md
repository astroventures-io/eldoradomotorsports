---
description: "When handling passwords, API keys, tokens, or other credentials during automation"
globs:
  - "**"
---

# Credential Hygiene — Never Expose Secrets in Conversation

## Password Entry via Playwright/Chrome

When entering passwords into browser forms:

1. **Write password to a temp file, read it, delete immediately:**
   ```bash
   source ~/.claude/scripts/bitwarden-helper.sh
   CREDS=$(bw_login_playwright "service")
   echo "$CREDS" | jq -r '.password' > /tmp/.pw_temp
   # ... use the file content to fill the form ...
   rm -f /tmp/.pw_temp
   ```
2. **NEVER pass passwords as plaintext tool arguments** — Playwright `type` and `fill` tool calls are visible in conversation history
3. **Use `slowly: true`** for password fields to handle JS validation
4. **Delete temp files immediately** after use — don't leave credentials on disk

## Why

A nestfolio session typed `@$tr0123!` as a direct Playwright tool argument, making it visible in the full conversation log. Passwords in tool call parameters persist in conversation history and session files.

## API Keys and Tokens

- **Doppler is the source of truth** — `doppler secrets get KEY --project X --config Y --plain`
- **Never hardcode** keys in scripts, CLAUDE.md, or commit messages
- **Never echo/print** secrets to stdout (they appear in bash tool output)
- **Mask when referencing:** `echo "Key: $(echo $KEY | head -c 5)..."`
- **Backend-only keys** (Stripe live, Discord tokens, Railway tokens) stay in Doppler only — never in `.env` files

## Session Cleanup

- `bw_lock` when done with a long automation session
- `rm -f /tmp/.pw_temp /tmp/.bw_session_*` to clean up credential files
