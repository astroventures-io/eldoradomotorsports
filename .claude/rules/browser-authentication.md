---
description: "When Claude encounters a login page, auth wall, or needs to access an authenticated service via browser"
globs:
  - "**"
---

# Browser Authentication — Use Bitwarden, Never Punt to Sophie

When you encounter a login page or authentication requirement during browser automation:

1. **NEVER say "I can't log in" or "you'll need to do this manually"** — credentials are in Bitwarden
2. **Source the helper:** `source ~/.claude/scripts/bitwarden-helper.sh`
3. **Search for credentials:** `bw_search "service-name"` to find the right item
4. **Get creds as JSON:** `bw_login_playwright "item-name (account)"` → `{username, password, totp, uri}`
5. **Fill the form** via Playwright or Chrome browser tools
6. **Handle 2FA:** If push notification (Google, Duo) → tell Sophie to approve on phone, then wait. If TOTP → `get_bw_totp "item"` for the code.

## Google OAuth (most common)

Many services use "Sign in with Google". Default account: `team@astroventures.io` unless Sophie specifies otherwise.

```bash
source ~/.claude/scripts/bitwarden-helper.sh
CREDS=$(bw_login_playwright "accounts.google.com (team@astroventures.io)")
```

## Password Security

- **NEVER type passwords as plaintext in Playwright tool arguments** — write to a temp file, read from it, delete immediately
- Use `slowly: true` for password fields
- Clean up: `rm -f /tmp/.pw_temp` after use

## If bw CLI Fails

The helper uses node@24 to work around the Node v25.6 readline crash. If it still fails:
- Check `doppler secrets get BITWARDEN_MASTER_PASSWORD --project global --config dev --plain` is set
- Check `/opt/homebrew/opt/node@24/bin/node` exists (`brew install node@24`)
- As last resort, ask Sophie to run `! bw unlock` interactively and paste the session key

## Skill Reference

Load `automation:playwright-login` from the skill registry for the full workflow documentation.
