# Failure log: AEL Desk Research Agent

**Group 3** · Developers: Tanishq Bhaiji, Nikola Hadzhiminov

Covers week 1 to the final review on 25 October. Entries are in the order they happened. Each entry records what we expected, what actually happened, why, and what changed in the platform as a result. Entries marked **open** are not fixed.

Referenced from: [PRD v1.1](week-06/AEL%20Product%20Requirements%20Document%20v1.1%20Group%203.pdf) and [Technical Blueprint v2.1](week-06/AEL%20Technical%20Blueprint%20v2.1%20Group%203.pdf).

---

## Week 1: throwaway build

### F1. English-only retrieval missed the best sources

Tested whether an agentic CLI could retrieve and translate a Dutch CBS page in one prompt. It could, roughly. The failure was in the retrieval, not the translation: searching only in English returned general material and missed the Dutch-language sources that actually cover a small Arnhem provider. Terms like *thuiszorg* and *wijkverpleging* have no search-equivalent English phrasing, so an English query does not reach the same documents.

**Changed:** dual-language retrieval went into the PRD as a hard requirement rather than a nice-to-have. Retrieve now runs 7 English and 6 Dutch terms on every run.

---

## Week 2: vendor jurisdiction research

### F2. The agent stated a regulatory deadline that had changed

Asked about EU AI Act Annex III high-risk obligations. The agent gave the deadline as 2 August 2026. It had been deferred to 2 December 2027 by the AI Omnibus (political agreement 7 May 2026, in force 27 July 2026). The wrong date was delivered in the same confident tone as everything else in the answer, with no hedging.

Caught by cross-checking against the European Commission's own AI Act page before the document went to the team.

**Changed:** regulatory claims are cross-checked against a primary source before use, not appraised on plausibility. This is the clearest evidence we have for why the Verify step exists and why a person does it.

### F3. Credible sources contradicted each other and the agent had no opinion

Searching NEN 7510 obligations returned sources in direct conflict. Several consultancy sites stated the standard is legally mandatory for care providers. NEN itself, and Dutch legal sources, describe it as enforced indirectly through the AVG and the Wabvpz, and triggered specifically when an organisation processes BSN and uses a care information system. Both sets of sources are findable, identifiable and recent.

The agent returned both and ranked them by its rubric. It did not notice the contradiction.

**Changed:** nothing in the agent, because this is not an agent problem. It confirmed that Appraise scores a source in isolation and cannot reconcile two sources against each other. That judgement sits with a person, and the appraisal file now carries a "not for" line per source so the limit is recorded rather than assumed.

### F4. A high-credibility source was wrong about a date

The Berkeley Technology Law Journal article dated the Latombe judgment to September 2024. EUR-Lex and InfoCuria both give 3 September 2025 (Case T-553/23). The most credible source in the set carried the error, and four lower-rated sources had it right.

**Changed:** nothing in the rubric. Its Reliability score (/20) rates who published a source, not whether each claim in it is correct, and the rubric has no separate accuracy score. We now treat a high Reliability score as a reason to read a source, not as proof of its claims: dates and legal facts are checked against a primary source at Verify (as in F2), and the "not for" line records what a source should not be cited for.

---

## Week 3: first build of the agent

### F5. Direct API calls are blocked in a published page

First working version called `https://api.anthropic.com/v1/messages` with `fetch`. It worked while developing and failed on the published page with "failed to fetch" at the Retrieve stage. Published artifacts cannot call that endpoint directly.

**Changed:** rebuilt against the `sample` capability, which is the supported route for a published page to call a model.

### F6. The agent invented plausible URLs

Asked for source URLs, it produced links that looked correct and did not resolve. It has no browsing, so it cannot check whether a link exists. A fabricated link is worse than no link, because it passes a quick glance and fails only when someone tries to open it.

**Changed:** the contract no longer asks for URLs. The model returns the publisher's real domain (which it does know) plus the search phrase a researcher would use. The page builds a site-scoped search link from those two fields, so every link works and lands the user on the source. Verification stays with the person, and the appraisal file says so.

### F7. JSON output was not reliably parseable

The model wrapped its JSON in markdown fences, or added a sentence before it, often enough to break a strict parse.

**Changed:** the parser strips fences and extracts the first array from the response, with a fallback so one malformed stage does not lose the whole run.

### F8. The agent worked for its author and for nobody else

Reported by Nikola, and independently by Valentina, who could not open the tool at all and had to sit with us to see it run. It worked on the author's account every time.

This took three attempts to diagnose and each one was wrong in an instructive way:

1. **First assumption: slow connection.** The capability check ran 300 ms after page load and permanently disabled the Run button if permissions had not arrived. The platform can take up to 10 seconds. Fixed the timeout to wait 20 seconds with retries. Still failed.
2. **Second assumption: a dismissed consent prompt.** Read the permissions contract properly and found the real gap: the page only ever tried to *use* the capability and never called `permissions.request()`, which is what shows the consent dialog. If a prompt was needed, none was ever shown, so it could never succeed. Also found that a denied permission stays denied for that page load, so the "Retry connection" button we had added was misleading in exactly the case it was meant to help. Added a real request call and state-specific messages. Still failed.
3. **Actual cause:** the artifact was first published with no capabilities declared, which pinned it to an older runtime. Every later publish added capability declarations but kept the old pin, so the capability layer was absent entirely. The diagnostic that gave it away was `permissions` itself resolving null, which should never happen because it is built in. Fixed by republishing with the runtime contract upgraded.

**Changed:** the page now reports the real permission state instead of guessing, and has a details panel that prints what it found so a teammate can send back something diagnosable rather than "it does not work".

**What this cost:** roughly two sessions of debugging, and two team members blocked from using the tool in the week it was meant to be tested. The root cause was invisible from the author's side because it worked on their own account. One person testing their own build is not a test.

---

## Week 5: LLM wiki (8 October)

### F9. The sandbox could not reach any source site (open)

`tools/fetch_article.py` returned 403 from the proxy for nen.nl, nedap-ons.nl and artificialintelligenceact.eu. Only GitHub was reachable. The fetch script needs the real page text, because the lint check compares quotations against the raw file character by character.

**Changed:** did not work around it. Building raw files from web-fetch summaries would have satisfied the file structure and made the quote check meaningless, which is the most valuable check in the system. Instead the wiki holds one source that could be obtained verbatim, and `INGEST-QUEUE.md` lists the rest in priority order to be fetched on a team machine.

**Open:** the queued sources are still not ingested. The wiki currently holds one source, which fails the Evidence gate, so it cannot yet support a claim in a handbook chapter. This is stated in the wiki's own synthesis page rather than left to be discovered.

### F10. Verified that the quote check actually fires

Not a failure, but a test that could have been one. Reworded a quotation on a source page from "grows faster than the value" to "always exceeds the value returned" and ran lint. It caught the altered quote and named the file. Restored the original and lint came back clean.

A check nobody has tested is not a check, and this is the mechanism that would catch an invented quotation.

### F11. Lint flagged an orphan page

Lint's orphan check reported the synthesis page: no other wiki page linked to it, so a reader moving through the concept pages would never reach it.

**Changed:** the synthesis is now linked from both concept pages it draws on. Lint then came back clean, with 0 errors and 0 warnings.

---

## Final review, 25 October (open)

### F12. Appraisal scores are matched to sources by title

Appraise returns one JSON row per source, and the page matches each row back to its source by title. If the model rewords a title, the row is not matched and that source is shown as 0/100 with "Evidence gate failed", with no warning.

**Mitigation until fixed:** at Verify, any source showing 0/100 is treated as unscored and the run is repeated, never used. **Fix:** match by position or an ID instead of by title.

### F13. The total is taken from the model, not recomputed

The model returns the five scores and a total. The page applies the Evidence gate and the bands in code, but uses the model's total without checking that it equals the sum of the five scores.

**Mitigation until fixed:** the Tester checks the sum in the score table at Verify. **Fix:** compute the total in code.

### F14. Sources are named from the model's memory

Retrieve cannot browse, so every source is one the model recalls. A recalled title, year or snippet can be wrong or invented, and the page does not check that the source exists.

**Mitigation:** this is why Verify opens every source through its search link before it is used, and why F6 removed model-made URLs.

### F15. The PDF export cannot print the arrow character

The PDF header line reads "Retrieve → Appraise", but the built-in PDF font cannot draw "→", so the exported file shows `!'` in its place on page one.

**Mitigation until fixed:** use the Markdown or Word export for files that will be handed in. **Fix:** replace the arrow with plain text ("Retrieve to Appraise") in the PDF builder.

---

## Patterns

Three lessons recur across the entries above.

1. **The agent fails plausibly.** F2 and F6 produced output that was wrong and looked exactly like output that was right. Nothing in the tone or format separates a wrong answer from a correct one, which is why a person verifies every source and every regulatory date.
2. **It scores sources; it does not reconcile them.** F3 and F4 were faults in the relationship between sources, not in any single source. Appraise rates each source alone, so contradictions and errors inside credible sources are a person's job.
3. **Testing on your own account is not testing.** F8 ran correctly for its author for as long as only its author ran it. A build counts as tested only once someone else has run it on their own account.

## Still open

- **F9:** the wiki holds one source; the queued sources still have to be fetched on a team machine.
- **F12, F13, F14:** checked by hand at Verify until fixed in code.
- **F15:** PDF export shows `!'` for the arrow; use Markdown or Word exports for hand-ins.
- **Band agreement:** the Tester's own bands for the eight-source comparison behind PRD assumption A3 are to be added to `appraisals/` as evidence.
