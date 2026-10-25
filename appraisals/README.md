# Appraisal files (run records)

One file per run of the [Desk Research Agent](../agent/), saved here after the Verify step.

## Naming rule

`appraisal-YYYY-MM-DD-<topic>.md` (or `.pdf` / `.docx`), for example `appraisal-2026-10-08-ai-scheduling-home-care.md`.

The exported file records the topic but **not the date or settings**, so:

- put the run date in the file name, and
- put the recency window and source count in the commit message, e.g. `Run 2026-10-08: ai scheduling home care, 5 yrs, 8 sources, verified by Valentina`.

## Before you commit a file

- [ ] Every source opened through its search link; sources that could not be found are struck through.
- [ ] No source shows 0/100 (if one does, the run was not fully scored: rerun).
- [ ] Each total equals the sum of its five scores.
- [ ] "Adopter hunt not run" noted at the top if stage 2 failed.
- [ ] Name of the person who verified the file in the commit message.
