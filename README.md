# CivicPlus Content Editor Tools

A Chrome extension (Manifest V3) that adds automation buttons to CivicPlus Central's Agenda Center and Archive Center bulk-upload pages, so content editors don't have to manually type in dates and descriptions for every file when uploading agendas, minutes, and archive items.

**[Install from the Chrome Web Store](https://chromewebstore.google.com/detail/auto-agenda-center/cbdnbmhhceadmbndeinmheallbmofago)**

## What it does

The extension injects a button directly into two CivicPlus admin pages:

- **Agenda Center → Multiple Upload Save Page**: adds an "Auto Name and Date Agendas" button. Click it, enter a description once (e.g. "Planning Board Regular Meeting Agenda (PDF)"), and it applies that description plus an auto-detected date — pulled from each PDF's filename — to every file in the batch.
- **Archive Center → Multiple Upload Save Page**: adds an "Auto Date Archive Items" button that does the same date auto-fill for archived files, without the description step.

Both rely on a shared date-parsing helper that recognizes a wide range of filename date formats (numeric like `03-14-2024`, spelled out like `March 14, 2024` or `Mar. 14th 2024`, etc.) and normalizes them to `MM-DD-YYYY`.

## How it works

| File | Role |
|---|---|
| `manifest.json` | Extension config (Manifest V3) — declares content scripts, icons, and host permissions scoped to `*/admin/AgendaCenter/MultipleUploadSavePage/*` and `*/admin/ArchiveCenter/MultipleUploadSavePage/*` |
| `scripts/agenda.js` | Injects the "Auto Name and Date Agendas" button on the Agenda Center upload page |
| `scripts/archive.js` | Injects the "Auto Date Archive Items" button on the Archive Center upload page |
| `scripts/dateFinder.js` | Shared `extractAndFormatDate()` helper — parses dates out of filenames using a set of regex patterns and a month-name lookup table |
| `scripts/background.js` | Minimal service worker (just logs on install) |
| `icons/` | Extension icons at various sizes |

## Installing for development

1. Clone this repo.
2. Go to `chrome://extensions` in Chrome.
3. Enable **Developer mode**.
4. Click **Load unpacked** and select the repo folder.
5. Navigate to a CivicPlus Central Agenda Center or Archive Center bulk upload page — the button will appear automatically.

## Notes

- Scoped via `host_permissions` and `matches` to only the two specific upload-page URL patterns, so it doesn't run anywhere else on a site.
- The date parser is filename-driven — it expects the date to be somewhere in the uploaded PDF's file name, and falls back to no match if it can't find a recognizable pattern.
