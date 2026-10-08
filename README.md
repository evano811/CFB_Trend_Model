# CFB Trend Model

**Working title: Saturday Game Guide**

A personal college football game companion that answers one question: *"What should I pay attention to in this game, and what evidence makes it interesting?"*

Before kickoff, it explains how two teams play, surfaces players worth following, and suggests specific things to watch. After the game, it helps compare those expectations with what actually happened.

> **Status: Week 1, discovery and repository foundations.** This repository currently contains planning and requirements documentation only. No application code, database, or data pipeline exists yet, and no data has been ingested. See [Current stage](#current-stage).

---

## Table of contents

- [Purpose](#purpose)
- [Reference matchup](#reference-matchup)
- [Core football questions](#core-football-questions)
- [Scope](#scope)
- [Current stage](#current-stage)
- [Repository layout](#repository-layout)
- [Documentation](#documentation)
- [Planned design](#planned-design)
- [Metric and evidence standards](#metric-and-evidence-standards)
- [Roadmap](#roadmap)
- [Working agreement](#working-agreement)
- [Setup](#setup)
- [Project board and issues](#project-board-and-issues)

---

## Purpose

The long-term aim is a way for **film, statistics, personnel and scheme** to come together to identify the most effective teams in college football, and to show how the phrase *"any given Saturday"* applies to particular matchups.

The product is built for curiosity and better viewing, not for betting. Odds, picks, bankroll tools and wagering features are explicitly out of scope, as are other commercial features.

**Success means** using the guide during at least three games, finding something new to watch in each, and keeping a brief postgame observation. Technical completeness alone is not the goal; the application should serve the viewing experience.

## Reference matchup

All early work is validated against a single completed game.

| | |
|---|---|
| **Game** | (17) Alabama at (5) Georgia |
| **Date** | September 27, 2025 |
| **Season** | 2025 regular season, Week 4 |
| **Venue** | Sanford Stadium |
| **Final score** | Alabama 24, Georgia 21 |
| **Teams** | University of Georgia Bulldogs, Alabama Crimson Tide |

Provider identifiers and the provider's week numbering are resolved from actual data before any dependent request is made. Provider IDs are never assumed to be interchangeable across sources (for example, ESPN and CFBD IDs).

## Core football questions

The first set of questions the project is designed to explore (more will follow with refinement):

1. How impactful are certain defensive schemes against offensive personnel matchups, and vice versa?
2. What were the most impactful types of plays, scenarios and outcomes in this game, and how do they extrapolate over the course of a season?
3. What primary defensive coverage or pressure package dictates a team's third-down success rate?
4. What real impact does weather have (rain, wind, temperature, wind direction, sun/shadows)?
5. Games feature a mix of mobile, scrambling and pocket quarterbacks. What coverages and techniques are used to mitigate these players?
6. Conversely, what formations, offensive schemes and/or player archetypes prove successful against each quarterback type?

Several of these (personnel, coverage, pressure packages) depend on data that may not be openly available. Whether they are feasible is exactly what the Week 1 validation work is meant to establish.

## Scope

### First release (minimum useful product)

- A weekly game list
- A matchup guide per game
- Comparable offensive and defensive tendencies for both teams
- Two or three players to watch per team
- Sourced coaching and scheme context, where available
- Kickoff weather
- Freshness labels showing how current each input is
- Personal postgame notes

Each guide follows a fixed reading order: game and kickoff details, why the matchup is interesting, when each team has the ball, players to watch, three keys or watching questions, weather, coaching context, sources and sample sizes, and personal observations. It should be readable on a phone, printable from the browser, and understandable without knowing EPA terminology.

Each "key" must include an observation to watch, a supporting metric or sourced qualitative claim, the comparison window, the sample size, and a limitation.

### Release acceptance

- Three selected games render end to end
- Every displayed statistic has a definition and denominator
- Every scheme assertion has a source or is labeled a personal observation
- Unavailable features display "unavailable" rather than invented values
- Refreshing an imported game does not create duplicates
- A cached guide survives provider downtime
- Pregame analysis excludes future information
- At least one game is watched using the guide and reviewed afterward

### Explicitly deferred

National coverage, live play-by-play, automated film classification, comprehensive injury feeds, route metrics, pressure metrics, personnel frequency charts, opponent-adjusted player rankings, ML predictions, and multi-user accounts. These enter the backlog only after their inputs are verified and their fan value is demonstrated.

## Current stage

**Week 1 (Oct 6-12, 2026): discovery and repository foundations.**

The immediate work is validating that accessible data can support the reference guide before any production ingestion is written (ticket **W1-04: Validate Data Sources**). That ticket covers:

- **Access and provenance:** working credentials stored privately, and a record of each request (source, endpoint, parameters, retrieval time, status, local sample path), access tier, and call count
- **Reference game identity:** confirming the Alabama/Georgia game record and the 24-21 final score
- **Play-by-play sample:** field mapping, inspection of representative plays and special cases (sacks, penalties, turnovers, kneels, spikes), and missing/null counts
- **Player statistics sample:** passing, rushing and receiving examples per team, and what can support production-based "players to watch" cards
- **Optional context checks:** venue coordinates, a weather sample, and whether personnel, formation, coverage, pressure or motion data exists in structured form
- **Deliverables:** a concise validation report and schema notes, a result table, reproduction steps, and a hand-off to W1-05 and W1-06

Neighboring tickets: **W1-05** maps football questions to the findings and decides feasibility; **W1-06** sketches the guide from supported inputs; **W1-07** (stretch) specifies a historical "Tale of the Tape" comparison.

Not required this week: a database, data pipeline, API server, computed impact score, national history import, live updates, dashboard, paid subscription, or automated scheme detection.

## Repository layout

```
CFB_Trend_Model/
├── README.md        Project entry point (this file)
├── .gitignore       Excludes OS noise and local virtual environments
├── docs/            Requirements, specification and planning documents
├── data/            Reserved for local data samples (nothing committed yet)
├── src/             Reserved for application code (nothing committed yet)
└── assets/          Reserved for project assets (nothing committed yet)
```

`data/`, `src/` and `assets/` are placeholders. Per the requirements, application directories, migrations, CI and deployment configuration arrive with later tickets, and unused services are not scaffolded in Week 1.

Raw provider responses will be kept **local and out of git** by default (e.g. `data/raw/`), and credentials are never committed.

## Documentation

| Document | Contents |
|---|---|
| [`docs/CFB_Repository_Requirements.md`](docs/CFB_Repository_Requirements.md) | Requirements baseline: Week 1 repository requirements (REP-01 to REP-08), planned product requirements (PROD-01 to PROD-09), working agreement, and the full W1-04 data-validation ticket with acceptance criteria and a report template |
| [`docs/Saturday_Game_Guide_Project_Specification.pdf`](docs/Saturday_Game_Guide_Project_Specification.pdf) | Product specification v1.0 (Oct 6, 2026): purpose, scope, data sources and feasibility gates, architecture, logical database model, metric definitions, read API, 10-week roadmap, backlog, and validation plan |
| [`docs/W1-01_ Reference Matchup & Workspace Setup Outline.pdf`](docs/W1-01_%20Reference%20Matchup%20%26%20Workspace%20Setup%20Outline.pdf) | Reference matchup, team selection, and the first set of core football questions |

## Planned design

Nothing below is built yet. It describes the direction set in the specification.

### Data sources (candidates)

| Source | Intended use | Notes |
|---|---|---|
| [CollegeFootballData (CFBD)](https://collegefootballdata.com/api-tiers) | Primary API candidate: games, teams, venues, rosters, player/team stats, plays, head-coach records | Free tier is documented at 1,000 calls/month; cache by endpoint and parameters; API keys stay server-side |
| [SportsDataverse / cfbfastR](https://cfbfastr.sportsdataverse.org/index.html) | Candidate bulk historical play-by-play source | Documented as an R package with prebuilt release assets; a Python project would evaluate the underlying release files |
| [Open-Meteo](https://open-meteo.com/en/pricing) | Weather | Free for noncommercial use within limits; forecast and historical weather are different datasets |
| Manual curation | Coordinator tenures and qualitative scheme context | CFBD documents head coaches only, not full OC/DC history; an OC title alone does not establish play-calling responsibility |

Documentation shows a *possible* capability; only a successful sample pull proves that a given account and season support it. CFBD's ordinary play response is documented to include situation, result, play text and PPA, but **not** per-play personnel, coverage, motion, routes, pressures or play action. Those features stay unverified or deferred until real data supports them. Provider terms are recorded before any raw data is redistributed.

### Proposed stack

Python (ingestion and analytics), PostgreSQL, FastAPI (small read API), and a React + TypeScript frontend, with Docker Compose for local services. Versions are pinned in lockfiles once chosen. This is a single application, not a set of microservices. Hosting and paid providers require a demonstrated need.

### Data flow

```
provider response → immutable raw snapshot → validation & identity mapping
  → normalized tables → versioned metric aggregates → guide evidence
  → read API → viewer
```

Raw payloads are retained with source, fetched-at timestamp, request parameters, schema version, and checksum. Timestamps are stored in UTC and displayed in the viewer's timezone. Entities use stable internal IDs with per-source mappings; players and coaches are never joined by name alone.

### Logical data model (summary)

Teams and seasons, venues, games, source-ID mappings, ingestion runs and raw snapshots, players and team memberships, per-game player stats, plays, coaches and tenures, scheme notes, weather snapshots, guide snapshots and keys, team metric aggregates, player watch candidates, and personal observations. Missing statistics are null/unavailable, never zero.

### Planned read API

`GET /health`, `/games`, `/games/{id}/guide?as_of=`, `/teams/{id}/profile`, `/players/{id}/profile`, `/coaches/{id}/history`, `/metrics/definitions`, and `GET`/`POST /games/{id}/observations` for private notes. These are application contracts, not CFBD endpoints.

## Metric and evidence standards

- A **metric dictionary** is written before any visualization. Each definition states eligible plays, exclusions, numerator, denominator, units, source, as-of cutoff, missingness handling, and version.
- Both sides of a comparison use identical definitions and windows.
- **Success rate** (project convention): at least 50% of yards to go on 1st down, 70% on 2nd down, and conversion on 3rd/4th down.
- **Early-down dropback rate**, **explosive rate** (runs of 10+ yards, passes of 20+ yards) and **stuff rate** use named, UI-visible thresholds. Sacks count as dropbacks.
- **EPA/PPA:** one verified provider metric is used under its actual name. CFBD's PPA and another source's EPA are never silently treated as interchangeable.
- **Windows:** season-to-date and last three completed games, ending strictly before the guide's `analysis_as_of`. Splits under 50 eligible plays get a low-sample label instead of ranked claims.
- **Players to watch** start as position-aware cards (QB, RB, receiver, defender), not a single opaque "Impact Score". Receptions are not targets; attempts are not snaps; sacks are not pressures.
- **No causal claims:** coordinator and weather comparisons describe team behavior during a window and do not attribute causal effects.
- **Missing vs. stale vs. zero:** unavailable data is shown as unavailable, and each guide records its last update and analysis cutoff.

## Roadmap

The specification plans ten one-week iterations at roughly six hours per week. Dates are provisional; acceptance gates control progression.

| Week | Dates | Focus |
|---|---|---|
| 1 | Oct 6-12 | Discovery and repository foundations; source validation |
| 2 | Oct 13-19 | Reliable ingestion slice and provider identity mapping |
| 3 | Oct 20-26 | Play normalization and metrics |
| 4 | Oct 27-Nov 2 | First usable viewer: game list and one-game guide |
| 5 | Nov 3-9 | Evidence-backed keys; mobile and print layout |
| 6 | Nov 10-16 | Coaching and scheme context |
| 7 | Nov 17-23 | Weather and freshness states |
| 8 | Nov 24-30 | Repeatable weekly refresh; expand to 4-8 teams |
| 9 | Dec 1-7 | Hardening, CI, backup/restore, optional private deployment |
| 10 | Dec 8-14 | Fan trials and release review |

Product-owner priority order: usable guide, then reliable facts, then understandable comparisons, then sourced context, then breadth.

## Working agreement

- **Roles:** the developer owns research, design, implementation, tests and deployment. The product owner clarifies scope, prioritizes tickets, and reviews evidence against acceptance criteria.
- **Board states:** Backlog, Ready, In Progress, In Review, Done. One substantive ticket at a time. Reported completion alone is not acceptance.
- **Branches and PRs:** short branches such as `w1-04-data-validation`; small PRs linked to the actual GitHub issue number (which may differ from the W1 label); closing keywords only when the PR completes the issue.
- **Cadence:** daily standup, Monday progress review, Monday retro/backlog.

## Setup

> Setup is minimal because there is no application code yet. This section will grow as tooling is chosen. Per requirement REP-08, tool versions are recorded as they are actually installed rather than assumed.

1. Clone the repository:
   ```bash
   git clone https://github.com/evano811/CFB_Trend_Model.git
   cd CFB_Trend_Model
   ```
2. Open the repository root in your IDE.
3. When a Python environment is configured, create it as `.venv/` at the repository root (already git-ignored).
4. When data validation begins, store API credentials **privately** (never in commits, screenshots, sample filenames or documentation) and save raw responses only to a git-ignored local location such as `data/raw/`.

**Tools and versions in use:** *not yet recorded; to be filled in during W1-02.*

## Project board and issues

- **Issues:** <https://github.com/evano811/CFB_Trend_Model/issues>
- **Board:** *link to be added once the project board is created* (REP-02 and REP-05 call for the README to link to it).

---

*This project is a personal college football fan project. It is not affiliated with any provider, school, or league, and it does not provide betting advice.*
