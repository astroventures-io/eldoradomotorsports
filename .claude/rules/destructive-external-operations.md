---
description: "When modifying content on external services (Notion, Super.so, Stripe, etc.) — prevent destructive replacements"
globs:
  - "**"
---

# Destructive External Operations — Count Before Replacing

When deleting or replacing content on external services (Notion pages, CMS blocks, database rows, etc.):

## Mandatory Pre-Check

Before any delete-then-replace operation:
1. **Count existing items** (e.g., image blocks on a Notion page)
2. **Count replacement items**
3. **If replacement < existing, STOP and flag it** — you're about to lose content

## Why

A nestfolio session deleted all Notion bedroom page images (5-10 each) and replaced with only 1-4 enhanced photos. The originals were file_upload images — once deleted from Notion, they're gone forever. Every room lost 50-80% of its photos.

## Rules

- **Notion image blocks:** Never "delete all, add new". Instead: match by filename, swap only the ones being replaced, preserve the rest.
- **Notion pages:** Before deleting blocks, export the block list. If anything goes wrong, you can reconstruct.
- **External file_upload content is NOT recoverable.** Only external URL images (hosted on Vercel/CDN) can be restored from git.
- **All future media should use external URLs** (deployed to `public/brand-assets/`), never Notion file_upload — this makes them recoverable.
- **Stripe charges/refunds:** Verify amounts before issuing. Refunds are immediate and cannot be un-refunded.
