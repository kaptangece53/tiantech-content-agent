# TIANTECH Weekly Intelligence — GitHub image handoff

## Architecture
Agent → GitHub PNG at `images/weekly-recap/<week_id>.png` → GitHub JSON `queue/weekly-recap.json` → weekly GitHub Actions → Make webhook → Make HTTP **Download a file** → LinkedIn company image post.

Dropbox is not used. Do not modify `queue/current.json` or the mini-guide workflow.

## Make configuration (manual one-time change)
1. Replace the Dropbox Download a File module in the **weekly recap** scenario with **HTTP > Download a file**.
2. Set HTTP URL to webhook field `119.image_url` (not `image_dropboxname`).
3. Map the downloaded HTTP file binary and `119.image_filename` to the LinkedIn company image-post module.
4. Re-determine webhook data structure if `image_url` is absent.
5. Verify one end-to-end run before treating this migration as operational. Do not modify the separate mini-guide scenario.

## Payload
- `image_url`: public immutable GitHub raw URL using the actual commit SHA, e.g. `https://raw.githubusercontent.com/kaptangece53/tiantech-content-agent/<40-character-commit-sha>/images/weekly-recap/2026-W45.png`.
- `image_filename`: exact PNG basename.
- `image_alt_text`: meaningful description.
- `image_dropboxname`: omit from new payload; it is deprecated.
- `status`: `READY_FOR_MAKE` only after image is uploaded and publicly retrievable.

## Publishing gates
1. Verify exactly three distinct, dated enterprise-relevant developments from the last 7 days, with valid sources and complete required metadata.
2. Validate Turkish LinkedIn copy: exactly 3 numbered developments, 1400–1900 characters (max 2000), exactly 4 hashtags including #TIANTECH and #TIANTECHINOVASYON, and an unattributed first-person closing paragraph under 'Bu haftanın ortak sinyali'.
3. Generate one final 1200×627 PNG using the TIANTECH palette.
4. Upload actual PNG bytes to `images/weekly-recap/<week_id>.png` via a binary-capable GitHub API (base64 blob/commit workflow or equivalent). Do not use UTF-8 text-file creation for binary PNGs. On conflicts use a unique timestamped filename. Confirm the image exists at its immutable commit SHA and a GET returns image/png.
5. Read current blob SHA of `queue/weekly-recap.json`; commit full valid JSON with `READY_FOR_MAKE`, `publish_company=true`, matching `image_filename` and `image_url`. If any gate fails, write a REVIEW_REQUIRED draft only, not the publication queue.
6. Confirm weekly GitHub Actions Validate payload, Verify GitHub image, and Send payload to Make steps succeed. Empty results from commit-workflow-runs lookup do not prove absence of push-triggered runs. Inspect alternative workflow runs/logs and fix failures with bounded retries while avoiding duplicate posts.
7. Confirm Make receipt and LinkedIn post separately when connector access allows. Never call Make directly.

## Migration note
The previous `queue/weekly-recap.json` may still contain the W44 Dropbox payload. Do not manually dispatch it until a new GitHub-hosted PNG and corresponding valid JSON are committed. The workflow now intentionally rejects the old Dropbox-only payload.
