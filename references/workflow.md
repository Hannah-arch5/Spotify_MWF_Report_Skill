# Spotify MWF Report Workflow

## 1. Orient

- Read `PROJECT_MEMORY.md`.
- Check `git status --short`, current branch, and whether a GitHub remote exists.
- Check LaunchAgent:
  - `launchctl print gui/501/com.hannah.spotify-podcast-report`
  - Confirm whether it is running or merely has an old nonzero exit code.
- Check recent service logs in `data/service_logs/`.
- Never restart from an old blocker if a newer memory section says delivery completed.

## 2. Determine Window And Manifest

- Prefer the project scripts and schedule logic already in the repo.
- For fixed windows, avoid overlapping reports. If a prior report included post-cutoff episodes, dedupe before generating the next report.
- Keep the manifest path explicit in all later commands.

## 3. Transcript Collection

- First run the project pipeline/evidence step to learn exactly which transcripts are missing.
- Correct STD workflow:
  - Use Chrome with the user's logged-in Spotify session and STD extension.
  - Open each Spotify episode detail page, not a search result page.
  - Click the native Spotify `Transcript` tab.
  - Wait for STD to download JSON files into `/Users/hannah/Downloads/Spotify Transcript Collector/`.
  - Use one tab sequentially if Spotify complains about too many open tabs.
- Import transcripts:
  - `scripts/import_spotify_transcripts.py`
- Rerun evidence/language audit and confirm required transcript coverage.
- Chinese transcript gaps can be reported, but English/original transcript coverage is the main generation gate unless the user requires Chinese.

## 4. Gemini Report

- Use the repo's pipeline scripts, usually chunked Gemini mode.
- If Codex requests approval because transcript/evidence will be sent to Gemini, ask/accept explicit user approval and do not bypass a rejection.
- After generation, run the Gemini review checker.
- If review says `需修改`, fix concrete findings and rerun review until `通过`.
- Convert suspicious non-verbatim quote blocks to explicit paraphrased conclusions rather than keeping them in quotation marks.

## 5. Render And Audit

- Render Word/PDF with `scripts/render_delivery_reports.py`.
- Do not accept lower-fidelity PDF fallback for final delivery unless the user explicitly approves it.
- Run `scripts/audit_delivery_report_format.py` on the DOCX and PDF.
- Confirm:
  - Expected main sections.
  - Episode heading count matches manifest.
  - Required labels match episode count.
  - PDF page count is plausible.
  - No audit issues.
- Record final DOCX/PDF paths and hashes.

## 6. Zotero

- If local Zotero DB writes are used, quit Zotero first when configured or when a lock is likely.
- Archive as direct PDF with `/unread` and month tag such as `/2606`.
- Verify local PDF hash matches active Zotero storage PDF with `scripts/verify_zotero_report_pdfs.py`.
- Record Zotero storage path and hash in memory.

## 7. External Delivery

- Stage files:
  - DOCX to `reports/archive/pending/<YYMM>/google-drive/`
  - PDF to `reports/archive/pending/<YYMM>/discord-todo/`
- Verify staged hashes match local finals.
- Google Drive:
  - Use configured rclone command.
  - Verify listing contains the uploaded DOCX.
- Discord:
  - Use `/Users/hannah/.discord-studio/Discord_Studio` as `SPOTIFY_DISCORD_CWD`.
  - Queue with `npm run send:discord -- <channel_id> <message> <pdf>`.
  - Verify live queue has `notification_sent`.
  - If an old queue path was used by mistake, mark old queued items as superseded to avoid future duplicate sends.

## 8. Cleanup And Mark Seen

- Always clean STD Downloads after report review and required delivery succeed:
  - `scripts/import_spotify_transcripts.py --move`
  - Verify `/Users/hannah/Downloads/Spotify Transcript Collector/` has zero JSON files.
- Mark manifests seen only after delivery gates pass:
  - `scripts/mark_manifest_seen.py <manifest>`
- If any required external delivery is blocked and not skipped by the user, do not mark seen.

## 9. Memory And Git

- Update `PROJECT_MEMORY.md` with:
  - Manifest, episode count, report paths.
  - Review/audit result.
  - Zotero path/hash.
  - Drive verification.
  - Discord `notification_sent` ids.
  - Transcript cleanup result.
  - Mark-seen result.
  - Any root cause found during debugging.
- For git:
  - Do not stage generated data blindly.
  - Separate code/docs/config changes from large generated artifacts.
  - If no remote exists, tell the user GitHub cannot be updated until a target repo is configured.

## 10. Final Response

- State what was completed and what remains blocked.
- Include concrete paths and verification results.
- If a delivery channel was blocked by approval policy, say so plainly and do not imply it succeeded.
