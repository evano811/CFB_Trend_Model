# CFB Trend Model: Repository Requirements

Version: 1.0 | October 7, 2026 | Product owner: ChatGPT | Developer: project owner

Suggested repository location: `docs/requirements.md`.
This document defines requirements. It is not a Python dependency file (`requirements.txt`).
Checkboxes describe work to verify; they do not assert that work is complete.

## Product purpose

Build a personal college football game companion that helps a fan understand matchups, follow players, and explore trends. The developer owns research, design, implementation and tests. The product owner clarifies scope, prioritizes tickets and reviews evidence. No betting recommendations or commercial features are in scope.

Reference teams: Alabama and Georgia. Reference game: September 27, 2025, Alabama at Georgia. Resolve the provider's identifiers and week from actual data before making dependent requests.

## Required now: Week 1 repository baseline

| ID | Requirement | Review evidence |
|---|---|---|
| REP-01 | One local clone opens at its root in the chosen IDE and points to the intended GitHub remote. | Repository URL, local setup description, successful push. |
| REP-02 | README explains purpose, reference matchup, current stage, setup steps and links to the board and this document. | README review. |
| REP-03 | `.gitignore` excludes local credentials, virtual environments, generated raw data and editor/system noise. | Reviewed diff and ignored-file check. |
| REP-04 | Documentation is versioned under `docs/`; local raw samples are retained separately from committed documentation. | File layout and sample inventory. |
| REP-05 | The board uses Backlog, Ready, In Progress, In Review and Done, with repository issues as committed work items. | Board and issue links. |
| REP-06 | Each active ticket has a goal, scope, observable acceptance criteria, dependencies and evidence requirements. | Issue descriptions. |
| REP-07 | Work is linked to issues through branches, commits, PRs or research-result comments. | One completed example. |
| REP-08 | Setup documentation records the tools and actual versions used, including unresolved setup steps. | Local setup notes; no assumption of preinstalled runtimes. |

Recommended initial locations:

- `README.md`: project entry point.
- `.gitignore`: exclusions.
- `docs/requirements.md`: this requirements baseline.
- `docs/reference-matchup.md`: selected game and football questions.
- `docs/data-validation.md`: W1-04 findings and request/sample inventory.
- `docs/feasibility.md`: W1-05 question-to-data mapping.
- `docs/wireframes/`: W1-06 sketches.
- `docs/tale-of-the-tape.md`: optional W1-07 discovery.
- `data/raw/`: local provider responses, initially ignored by git.
- `.venv/`: local Python environment when configured, ignored by git.

Create locations as their work starts. Application directories, database migrations, CI and deployment configuration follow later tickets. Do not scaffold unused services for Week 1.

## Working agreement

Work on one substantive ticket at a time. Ready means its inputs and acceptance criteria are understood. In Review means deliverables and evidence are available. Done means the criteria have been met and reviewed. Reported completion alone is not acceptance.

Use short branches such as `w1-04-data-validation`. Review changes before committing; push useful increments. PR descriptions explain the result and validation. Reference the actual GitHub issue number, which can differ from the W1 ticket label. Closing keywords are used only when the PR completes the issue. Research work may close through accepted findings without an implementation PR.

Daily standup: completed, next task, blockers and available time. Monday progress review: evidence, criteria, hours and scope. Monday retro/backlog: process changes and next priorities. Friday October 9 is a target for the first sprint; unresolved core work can carry forward. W1-07 is a stretch discovery ticket.

## Planned product requirements: later implementation

| ID | Requirement | Completion condition |
|---|---|---|
| PROD-01 | A selected matchup guide presents both teams, kickoff/status, tendencies and players to watch. | One actual-data guide usable on a phone. |
| PROD-02 | Metrics are comparable and explainable. | Definitions, windows, units, exclusions and denominators are visible or linked. |
| PROD-03 | Watching questions and keys have traceable evidence. | Each claim identifies supporting data or sourced observation and its limitations. |
| PROD-04 | Missing and stale information is explicit. | Unavailable is distinct from zero; last update and analysis cutoff are recorded. |
| PROD-05 | Historical guides avoid future information. | Inputs end at the guide's analysis cutoff. |
| PROD-06 | Personal postgame observations are retained. | Notes remain linked to the relevant game and optional watching question. |
| PROD-07 | Imports are repeatable and identity-safe. | Reruns do not duplicate facts; provider corrections are handled in a later ingestion ticket. |
| PROD-08 | Coaching/scheme assertions are sourced. | Roles, dates and uncertainty are recorded; ordinary statistics are not mislabeled as coverage charting. |
| PROD-09 | Tale of the Tape compares separate historical windows. | 2024, 2025 and 2026-to-date are not pooled silently; personnel changes have verified context. |

Proposed later stack: Python, PostgreSQL, FastAPI and a TypeScript frontend. Versions and dependency tooling will be selected collaboratively. Hosting and paid providers require a demonstrated need; neither is a Week 1 requirement.

## W1-04: Validate Data Sources

**Goal:** Prove that accessible data can support the reference guide and identify its limits before writing the production ingestion pipeline.

**Dependency:** W1-01 accepted; W1-02 local workspace ready for saving findings.

**Estimate:** Two hours, including documentation. If blocked for about 30 minutes by access or schema problems, bring the error and attempted steps to review.

**Scope:** One reference matchup; CFBD is the primary API candidate. Documentation playground, a manual request, or your own small exploratory script are acceptable. A script is optional. Inspect actual responses rather than treating documentation as proof of access.

### A. Access and provenance

- [ ] AC-01: Obtain working credentials where required and store them privately. No keys appear in commits, screenshots, sample filenames or documentation.
- [ ] AC-02: Record source, endpoint/dataset, nonsecret parameters, retrieval time with timezone, HTTP/result status and local sample path for each attempted dataset.
- [ ] AC-03: Record the access tier used, applicable request limit and number of calls made during validation. Documentation-only capabilities are marked unverified.

### B. Reference game identity

- [ ] AC-04: Retrieve an actual game record matching Alabama at Georgia on September 27, 2025. Record provider game ID, season, provider week, season type, home/away identities, completion status and available venue/kickoff information.
- [ ] AC-05: Confirm the final score is Alabama 24, Georgia 21. Record any discrepancy rather than editing the raw response to fit expectations.
- [ ] AC-06: Distinguish provider game/team/player IDs from names and IDs used by other sources. Do not assume ESPN and CFBD IDs are interchangeable.

### C. Play-by-play sample

- [ ] AC-07: Retrieve the reference game's play records, or a documented broader response that can be filtered to its verified game ID. Record raw count and retained game count separately.
- [ ] AC-08: Inspect at least ten representative plays, including both offenses and multiple downs. Locate sacks, penalties, turnovers or other special cases if present; record absent examples as absent rather than inventing them.
- [ ] AC-09: Map exact response fields for game/play identity, offense/defense, period/clock, down, distance, play type, yards gained and score context. Flag missing fields and fields requiring interpretation.
- [ ] AC-10: Identify whether a provider efficiency value exists, its exact name, null handling and methodology reference. Do not silently rename PPA as another source's EPA.
- [ ] AC-11: For required situation/result fields and the efficiency field, report missing/null counts over the retained game records. Empty strings and invalid values are noted separately where relevant. A field absent entirely is unavailable, not zero.
- [ ] AC-12: Document which special cases could affect later metrics: sacks, scrambles, no-plays, penalties, kneels and spikes. Week 1 requires a classification proposal and uncertainties, not a completed normalization engine.

### D. Player statistics sample

- [ ] AC-13: Retrieve actual reference-game player statistics for both teams. Identify the response nesting/category structure and provider player identifiers.
- [ ] AC-14: Locate at least one passing, rushing and receiving example per team where supplied. Record exact fields, units and whether values are strings or numbers.
- [ ] AC-15: Explain which records can support production-based players-to-watch cards. Mark targets, routes, snaps, pressures and usage as unavailable or unverified unless actual fields support them. Receptions are not substituted for targets.

### E. Optional/context data checks

- [ ] AC-16: Inspect venue coordinates/timezone availability and attempt one weather sample if accessible within the timebox. Record forecast versus historical type, valid time and units. Missing access or venue inputs may be accepted with an evidenced fallback/defer decision.
- [ ] AC-17: Check whether available samples contain personnel groups, formations, coverage, pressure packages, play action or motion. For each, mark verified, unverified or unavailable. Free text alone does not establish trustworthy structured charting.
- [ ] AC-18: Record the provider terms/access documentation consulted and any unresolved permission question for sample retention or sharing. Keep raw responses local initially; public sample publication requires a separate decision.

### F. Deliverables and acceptance decision

- [ ] AC-19: Save original sample responses locally, or preserve permitted exports, with a documented inventory. Commit a concise validation report and schema notes; exclude credentials and raw datasets from the PR by default.
- [ ] AC-20: Include a result table with: dataset, actual request, status, count, key fields, missingness, limitation, supported feature and fallback.
- [ ] AC-21: Demonstrate how another run could reproduce the samples using privately configured credentials. Clear manual steps are sufficient; automated refresh is not required.
- [ ] AC-22: Provide an issue comment or PR linking findings and identifying what W1-05 and W1-06 can now use. List remaining blockers and one deferred feature.

**Done:** Actual reference game, play and player-stat samples pass the core criteria; optional context gaps have evidence and fallback decisions; findings are reviewed. If core data cannot be obtained, submit a useful blocked discovery report. The product owner must explicitly accept an alternative source or revise scope before the ticket is Done.

**Not required:** Database, data pipeline, API server, computed impact score, national history import, live updates, dashboard, paid subscription or automated scheme detection. Missingness inspection is required; production analytics and automated tests are later work.

### Validation report template

| Dataset | Request/filters | Result/count | Verified fields | Missingness | Feature supported | Limitation/fallback |
|---|---|---|---|---|---|---|
| Game metadata | Fill from actual request | Pending | Pending | Pending | Matchup identity | Pending |
| Plays | Fill from actual request | Pending | Pending | Pending | Situation comparisons | Pending |
| Player stats | Fill from actual request | Pending | Pending | Pending | Players to watch | Pending |
| Venue/weather | Fill from actual attempt | Pending | Pending | Pending | Game context | Optional fallback |
| Scheme/personnel | Fill from actual inspection | Pending | Pending | Pending | Later charted analysis | Defer if unsupported |

## Next-ticket boundaries

W1-04 records what data exists and how it behaves. W1-05 maps football questions to those findings and makes feasibility decisions. W1-06 sketches the guide from supported inputs. W1-07 specifies the historical comparison and roster-change feature; it does not require implementing it this sprint.
