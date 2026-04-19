---
description: "When working with Supabase migrations, database connections, or RLS policies"
globs:
  - "supabase/**"
  - "**/*.sql"
  - "**/supabase*"
  - "src/lib/supabase/**"
---

# Supabase Safety — Migrations, Connections, and RLS

1. **NEVER use transaction pooler** (port 6543) — causes SASL auth failures. Session pooler (port 5432) only.
2. **Always use `SUPABASE_DB_URL`** from Doppler, not `DATABASE_URL`.
3. **Run `verify-supabase` before any migration** to confirm correct project connection.
4. **Test migration locally first:** `supabase db reset` (local only, NEVER `--linked`).
5. **RLS policies:** Every new table needs RLS enabled. Every policy must reference `auth.uid()` or be explicitly public with a comment explaining why.
6. **Naming:** Migration files use `YYYYMMDDHHMMSS_description.sql` format. Be descriptive.
7. **Supabase MCP** uses remote HTTP transport (`mcp.supabase.com`) with `SUPABASE_ACCESS_TOKEN` (PAT). Plugin disabled — PAT avoids OAuth re-auth.
8. **Never `supabase db reset --linked`** — this nukes the remote database. Local resets only.
