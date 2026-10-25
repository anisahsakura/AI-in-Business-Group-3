# Desk Research Agent

Group 3's source-gathering and appraisal tool: **Retrieve → Adopter-case hunt → Appraise → Export**, in Dutch and English, with a 100-point rubric and an Evidence gate.

- **Run it:** [claude.ai/artifact/91rBNEMcY5xrnnbCr3Mw13](https://claude.ai/artifact/91rBNEMcY5xrnnbCr3Mw13)
- **Code:** [`desk-research-agent.html`](desk-research-agent.html), a copy of the agent's code taken on 25 October 2026. If the published page is changed, replace this copy and `prompts.md` in the same commit.
- **Prompts and rubric:** [`prompts.md`](prompts.md)
- **Design:** Technical Blueprint v2.1 and PRD v1.1 in [`../final/`](../final/)
- **What went wrong while building it:** [`../failure-log.md`](../failure-log.md)

## How to run it

1. Open the link while signed in to claude.ai, in a desktop browser.
2. Press **Run search strategy** and choose **Allow** when asked for model access. If you decline, reload the page: a declined permission stays declined for that page load (failure log F8).
3. Enter a topic, or click one of the seven preset topics. Choose the recency window (3, 5 or 10 years) and the number of sources (6, 8 or 10). The context box is optional; put **public facts only** in it.
4. Wait for all three stages to turn green. A red stage shows its error; see *When a stage fails*.
5. Export: **Copy markdown**, **PDF** or **Word (.docx)**.

If it does not start, press **Show details** under the button and send that text to a Developer.

## After every run (manual steps; the agent does not do these)

1. Save the export in [`../appraisals/`](../appraisals/) using the naming rule there.
2. **Verify:** open every source through its search link and confirm it exists and says what the appraisal claims. Strike any source that cannot be found. The model names sources from memory and cannot browse (F14).
3. Treat any source showing **0/100** as unscored (F12) and check that each total equals the sum of its five scores (F13).
4. If stage 2 failed, write "adopter hunt not run" at the top of the file.

## When a stage fails

| Stage | What you see | What to do |
|---|---|---|
| 1 Retrieve | Red; the run stops | Rerun up to twice, then search by hand with the week-3 search strategy |
| 2 Adopter hunt | Red; the run continues with no adopter section | Note "adopter hunt not run" in the file |
| 3 Appraise | Red; sources shown unscored (0/100) | Rerun; never use that file |

## Manual fallback

Every step can be done by hand: search with the terms in `prompts.md`, score with the same rubric and gate, and write the appraisal file in the same format.

## Rules

- No URLs written by the model. Links are built from the publisher's domain plus a search phrase (F6).
- Never paste interview material or anything identifying a Thuiszorg Totaal client or employee into the context box.
- If you change the page, update `prompts.md` and this copy of the HTML in the same commit.
