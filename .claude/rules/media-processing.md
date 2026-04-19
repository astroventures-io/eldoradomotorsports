---
description: "When processing photos, images, or media files for web deployment or external services"
globs:
  - "**/*.jpg"
  - "**/*.jpeg"
  - "**/*.png"
  - "**/*.heic"
  - "**/*.webp"
  - "public/brand-assets/**"
  - "**/photos/**"
  - "**/images/**"
---

# Media Processing — EXIF, Compression, and Recoverability

## iPhone Photos

iPhone saves photos with EXIF orientation metadata instead of rotating pixels. Without correction, portrait shots appear sideways on web.

- **Always EXIF transpose** before web deployment: `ImageOps.exif_transpose(img)` in Pillow, or `sips` handles it automatically for HEIC→JPG
- **HEIC→JPG conversion:** Use `sips --setProperty format jpeg` on macOS, or Pillow with `pillow-heif`

## Compression Before Upload

- **Before sending to APIs** (AI analysis, form uploads): compress to max 800px wide, JPEG quality 0.6 (~50-100KB each)
- **For web deployment** (Vercel, CDN): max 1920px wide, JPEG quality 0.85
- **Never send raw iPhone photos** (5-10MB each) as base64 in API requests — will hit body size limits

## Recoverability

- **Always use external URLs** (hosted on Vercel/CDN in `public/brand-assets/`) instead of platform file_upload (Notion, etc.)
- External URL images are recoverable from git. Platform file_upload images are gone forever once deleted.
- When replacing images on external services, keep the originals in git — don't delete the source files.

## Naming Convention

- Enhanced/processed photos: `01-description.jpg`, `02-description.jpg` (sort first)
- Originals added later: continue numbering from where enhanced left off (`04-description.jpg`)
