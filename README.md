# Spotify MWF Report Skill

Codex skill for Hannah's Monday/Wednesday/Friday Spotify podcast intelligence report workflow.

This skill preserves the operating checklist for producing recurring Spotify podcast intelligence reports, including transcript collection, report generation, review, rendering, Zotero archiving, Google Drive upload, Discord delivery, transcript cleanup, and final mark-seen handling.

## What It Covers

- Reads the project memory before continuing work.
- Checks git state, LaunchAgent state, and recent service logs.
- Collects missing Spotify transcripts through the STD Chrome extension.
- Enforces the correct STD workflow: open the Spotify episode detail page and click the native `Transcript` tab.
- Imports transcript JSON files into the project archive.
- Generates and reviews the report with the project pipeline.
- Renders final Word/PDF deliverables and audits formatting.
- Archives PDFs to Zotero and verifies hashes.
- Uploads Word files to Google Drive.
- Sends PDFs to Discord through the live Discord Studio queue.
- Cleans `/Users/hannah/Downloads/Spotify Transcript Collector/` after successful delivery.
- Marks manifests seen only after required delivery gates pass.
- Updates project memory at the end.

## Install

Copy this repository folder into:

```bash
/Users/hannah/.codex/skills/spotify-mwf-report
```

In this setup it is already installed locally at that path.

## Trigger Phrases

Use any of these in a Codex thread:

- `做今天 Spotify 播客研报`
- `继续周三 Spotify 研报流程`
- `跑 MWF Spotify report skill`
- `检查 STD transcript 并交付研报`

## Important Paths

- Project: `/Users/hannah/Documents/Spotify All in One`
- Project memory: `/Users/hannah/Documents/Spotify All in One/PROJECT_MEMORY.md`
- STD downloads: `/Users/hannah/Downloads/Spotify Transcript Collector/`
- Live Discord Studio: `/Users/hannah/.discord-studio/Discord_Studio`

## Notes

The skill is intentionally strict about validation. It should not silently use lower-fidelity fallbacks, skip transcript cleanup, or mark episodes seen before delivery is complete.
