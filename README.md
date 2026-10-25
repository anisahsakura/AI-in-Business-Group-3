# AI in Business – Group 3

HAN minor *Data Driven Decision Making in Business* · AIBS (research) and AEL (agent engineering) · September–October 2026

**Team:** Anisah Sakura Sutrisno (Product manager) · Tanishq Bhaiji and Nikola Hadzhiminov (Developers) · Valentina Petrušić (Tester) · Tanjin Rahman Rafi (Deployer)

**SME:** Thuiszorg Totaal, a home-care provider in Arnhem (~70 staff). Use case studied: AI-supported advance notice of caregiver visits. The field interview did not take place, so the handbook rests on desk research and the SME's problem remains a hypothesis.

## Start here

| What | Where |
|---|---|
| Final PRD v1.1 and Technical Blueprint v2.1 | [`week-06/`](week-06/) |
| Handbook, four themes (latest version, 9 October) | [`week-05/AIBS_Handbook_Team3_0910.pdf`](week-05/AIBS_Handbook_Team3_0910.pdf) |
| Desk Research Agent: link, prompts and rubric, how to run | [`agent/`](agent/) |
| LLM wiki | [`wiki/`](wiki/) |
| Appraisal files (one per agent run) | [`appraisals/`](appraisals/) |
| Failure log, week 1 to the final review (F1–F15) | [`failure-log.md`](failure-log.md) |
| Individual portfolios (all five team members) | [`Individual Portfolios/`](Individual%20Portfolios/) |

## Folders

| Folder | Contents |
|---|---|
| `week-01/` | First drafts: PRD and research proposal |
| `week-02/` | PRD, technical blueprint, research proposal, handbook, search strategy v1 |
| `week-03/` | Handbook (Theme 1 + Theme 2), search strategy v2, interview guide, updated PRD |
| `week-05/` | Handbook with all four themes (versions of 8 and 9 October) |
| `week-06/` | Final documents: PRD v1.1 and Technical Blueprint v2.1 (25 October), Word and PDF |
| `Individual Portfolios/` | The five individual portfolios (Word): Anisah, Tanishq, Nikola, Tanjin, Valentina |
| `agent/` | Desk Research Agent: link to the published page, prompts and rubric, run guide |
| `wiki/` | LLM wiki files and status |
| `appraisals/` | Exported, verified appraisal files with naming rule |

## The platform in one paragraph

The Desk Research Agent retrieves sources in Dutch and English, hunts for an independent case of a comparable organisation that actually used such a system, and appraises every source on a 100-point rubric with an Evidence gate. People then verify every source, store verified sources in the LLM wiki, draft the handbook page with model help, check it against yes/no criteria and sign it off before it is published. Interview and client data never reach a model service. Every automated step has a manual fallback.
