---
name: spotify-mwf-report
description: Run or continue Hannah's Monday/Wednesday/Friday Spotify podcast intelligence report workflow. Use when the user asks to make today's, Monday, Wednesday, Friday, or MWF Spotify podcast report; continue Spotify report automation; collect STD transcripts; deliver the report to Zotero, Google Drive, and Discord; debug LaunchAgent/report delivery; or ensure transcript cleanup and mark-seen completion.
metadata:
  short-description: Run Spotify MWF podcast report workflow
---

# Spotify MWF Report

Use this skill for Hannah's recurring Spotify podcast intelligence report workflow.

## Start Here

1. Work in `/Users/hannah/Documents/Spotify All in One`.
2. Read `PROJECT_MEMORY.md` first. Trust the newest dated section over older blocker notes.
3. Check current state before doing heavy work:
   - `git status --short`
   - `git branch --show-current`
   - LaunchAgent state for `com.hannah.spotify-podcast-report`
   - latest `data/service_logs/*.json`
   - `/Users/hannah/Downloads/Spotify Transcript Collector/` JSON count
4. Follow the full workflow in `references/workflow.md`.

## Hard Rules

- Do not silently use lower-fidelity transcript/report fallbacks.
- For STD transcripts, open each Spotify episode detail page and click Spotify's native `Transcript` tab. Search/list pages are not enough.
- After report review passes and delivery succeeds, always run transcript cleanup with `scripts/import_spotify_transcripts.py --move` and verify Downloads has zero leftover JSON files.
- Mark episodes seen only after the required delivery gates pass, unless the user explicitly skips a blocked channel.
- Discord delivery must use the live Discord Studio directory: `/Users/hannah/.discord-studio/Discord_Studio`.
- When sending transcript/evidence to Gemini or files to Google Drive/Discord, respect Codex approval prompts. If approval is blocked, report it directly and do not work around it.

## Common User Triggers

- “做今天 Spotify 播客研报”
- “继续周三 Spotify 研报流程”
- “跑 MWF Spotify report skill”
- “检查 STD transcript 并交付研报”
- “调试 LaunchAgent / Drive / Discord / Zotero”
