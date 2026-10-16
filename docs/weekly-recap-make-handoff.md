# TIANTECH Weekly Intelligence — Dropbox path handoff

Make scenario: Webhook (119) → Dropbox Download a File (127) → LinkedIn company image post.

## Required payload fields
- `image_dropboxname`: exact absolute Dropbox-root file path, e.g. `/TIANTECH-WEEKLY-RECAP-2026-W42.png`.
- `image_filename`: basename of `image_dropboxname`.
- `image_url`: omit or set to empty string; it is not used by this Make scenario.
- `status`: `READY_FOR_MAKE` only after Dropbox upload and all content checks succeed.

## Make configuration
Dropbox module 127: **Download a File**, **Way of selecting files = Map a file path**, **File Path = 119.image_dropboxname**. LinkedIn image input should map Dropbox module 127's downloaded file data and filename, not a temporary Dropbox URL.

## Agent publishing gate
1. Research exactly three distinct, dated and verified developments within the previous seven days.
2. Validate Turkish post length, four hashtags, practitioner closing paragraph and source URLs.
3. Generate final PNG and upload it to Dropbox root. On conflict use unique generated_at suffix.
4. Confirm upload succeeded and exact Dropbox path returned. Set `image_dropboxname` to that path and synchronize `image_filename`.
5. Read current blob SHA for `queue/weekly-recap.json` and commit complete payload to main with `READY_FOR_MAKE` only if all gates passed. Otherwise keep publication queue unchanged and save a REVIEW_REQUIRED draft.
6. Never request temporary download URLs, HEAD/fetch them, or call Make directly.
7. Do not modify `queue/current.json` or the mini-guide workflow.

**Note:** Merely changing the scheduled-agent prompt does not change Make's scenario. The webhook module must expose `image_dropboxname` (redetermine data structure if needed), and the Dropbox file module must use that field.
