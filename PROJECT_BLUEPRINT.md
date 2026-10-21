# SFC Opava — Recruitment Intelligence
## Project Blueprint | v1.0 | 21 October 2026

**Purpose:** Independent application portfolio for **Mario Rodenas Sistané**, applying for a **Data Scout / Recruitment Analyst** opportunity. The app is a recruitment analysis demonstration, **not an official SFC Opava product**.

> Status convention: **Approved**, **Designed / not yet implemented**, **Pending verification**.

## 1. Product principles

- **Language:** English throughout the application; preserve Czech proper names.
- **Stack:** Python, Streamlit, VS Code on Windows; source data in Excel; individual reports as PDF.
- **Design:** Football Manager-inspired layout, not a literal clone. Dark navy base (`#0B1424`), club blue (approximately `#00549F`) and yellow (`#FFE500`), clear typography, compact panels, wide tables and restrained charts.
- **Integrity:** No invented player attributes, scouting opinions, transfer values, salaries, contract situations, market values, or performance scores. Display only sourced data and clearly explained calculations.
- **Navigation:** Club Overview · Squad Analysis · Market Scouting · Player Comparison · Recruitment Reports.
- **Attribution:** Small, single-line applicant banner: `DATA SCOUT / RECRUITMENT ANALYST · APPLICATION PORTFOLIO` and `MARIO RODENAS SISTANÉ` (Rodenas **without** accent; Sistané with final acute accent).
- **Existing homepage is approved:** Do not redesign or overwrite its content without explicit request. Preserve the current working `app.py` and assets.

## 2. Data foundation

- Source: **SofaScore**, as confirmed by the user.
- User-provided workbook: two sheets, including consolidated **`Base scouting`** sheet. Previous review reported **390 player records, 16 clubs, 25 columns, 21 SFC Opava records**; **revalidate against the actual workbook** when supplied for implementation.
- Core dimensions: player, club, position, appearances, starts, minutes; performance variables include goals, shots, assists, key passes, passing, dribbling, duels and defensive actions as available.
- Data preparation: validate names and clubs; normalize positions; distinguish totals, per-90 rates and percentages; verify percentage scaling (e.g. `0.8125` = `81.25%`); preserve source and season metadata; handle missing values honestly.
- **Per-90 formula:** `stat / minutes * 90`, only when minutes > 0. Percentage metrics remain percentages.
- Compare like positions and use minimum-minutes thresholds. Percentiles must be defined against a relevant comparison group; do not interpret small samples as equivalent to regular starters.
- Disciplinary leaders only if yellow/red-card data exist. Age, nationality, player photo, dorsal, transfer/contract data only if available and verified.
- Confirm exact season and competition coverage before publishing a season label on analytic pages.
- The two analytical groups are **SFC Opava squad** and **other clubs in the same league**; a single unified player table powers both.

## 3. Pages

### 3.1 Club Overview — APPROVED / ALREADY IMPLEMENTED

- Keep the existing approved landing page unchanged.
- Club: **Slezský fotbalový club Opava a.s.** / **SFC Opava**, founded 1907, Opava, Czechia.
- Existing design includes crest, competition(s), stadium details/photo and compact application identification. Retain current verified images/logos in `assets/`.
- Do not add extra charts, financial indicators or clutter to the homepage.

### 3.2 Squad Analysis — DESIGNED

**Layout:** Football Manager-inspired tactical pitch and roster table, with compact squad leaders below.

- **5-3-2 formation** visualization, populated from actual SFC Opava records.
- Label: **`Most-used XI · 5-3-2 (estimated)`**. Select players by minutes and position compatibility, without duplication; this is **not** proof of the manager's most frequent actual XI (requires match-by-match lineups).
- On-pitch cards: name, position, minutes, dorsal only if known. Optionally select player to inspect available stats.
- Full roster table: name, position, appearances, starts, minutes, goals, assists and other available columns; sortable and position-filterable.
- **Squad Leaders:** most goals, assists, minutes, shots, key passes and yellow/red cards if available.
- Optional restrained squad profile (minutes by position, mean age if ages are available).
- No invented tactical roles or subjective quality stars.

### 3.3 Market Scouting — DESIGNED

**Layout:** Wide Football Manager-style searchable player table, with a prominent **Advanced Filters** button opening a **modal dialog** (explicit user decision).

- Search by player name; total/filtered result count; change table view: Overview, Attacking, Passing, Dribbling/Duels, Defending (and Goalkeeping only if source metrics support it).
- **Modal advanced filters:** position, age if available, minimum minutes, team/exclude Opava, and **multiple cumulative statistical conditions** with threshold and metric selector.
- Buttons **Apply** and **Reset**; persist applied filters across table interactions.
- Sortable table, totals/per-90 toggle where meaningful, percentage columns correctly formatted.
- Select two players and send to Player Comparison. Avoid imaginary ratings, scouting knowledge, salary, value, or contract data.
- Potential future fit score only if the user explicitly approves a transparent metric definition and weights.

### 3.4 Player Comparison — DESIGNED

**Layout:** Two side-by-side player headers and two views: **Overview** and **Detailed Statistics**.

- Modes: SFC Opava vs other club; other club vs other club; SFC Opava vs SFC Opava.
- Pick two unique players from the unified database; allow selection passed from Market Scouting.
- **Overview:** name, club, position, age if available, minutes, key metrics and a 6–8-axis radar **only for metrics with sufficient valid data**.
- Radar uses positional percentiles or another clearly explained normalized scale; categories must reflect **actual available metrics** (not invented 'mental', 'physical', 'potential', etc.).
- **Detailed Statistics:** side-by-side comparisons grouped by Attack, Passing, Dribbling, Duels, Defense and Discipline (only if columns exist); totals and per-90 where applicable.
- Indicate low-minute sample sizes and missing data; do not present differences as causal conclusions.

### 3.5 Recruitment Reports — DESIGNED

**Layout:** Intentionally minimal: **2–3 recommended players**, each as a compact card with name, club, position, age if available, short recruitment rationale and **View / Download PDF** action.

- PDFs will be supplied by the user after candidates are selected. Do not fabricate recommendations before evaluating data and context.
- Reference example: user's **Josep Cerdà** one-page scouting PDF: player header/photo, short description, radar, league percentiles, positional diagram, career path and conclusion. This is **format inspiration**, not automatically a recommended SFC Opava signing.
- Reports should conclude the workflow from squad needs → market screening → comparisons → recommendations.

## 4. Delivery and implementation workflow

1. **Back up** the approved app and existing `assets/` before edits; never overwrite the sole working copy blindly.
2. Place the real consolidated Excel in a project `data/` directory; inspect its **exact** column names, datatypes, nulls, teams, position vocabulary, season and coverage.
3. Build reusable loading, validation and metrics utilities, ideally separate from the Streamlit UI.
4. Implement and test **Squad Analysis** against real data.
5. Implement **Market Scouting**, including the modal filters and sortable database.
6. Implement **Player Comparison**, linked to the Market Scouting selections.
7. Add user-provided **Recruitment Reports** PDFs and recommendation cards.
8. Validate UX, mobile/desktop layout, correctness of derived statistics, missing-data behavior, and sources; deploy only after user approval.

## 5. Suggested repository structure (future; do not reorganize blindly)

```text
sfc_opava_recruitment/
├── app.py                      # Existing approved app / entry point
├── requirements.txt
├── README.md
├── PROJECT_BLUEPRINT.md        # This specification
├── assets/                     # Existing club crest, competition and stadium assets
├── data/                       # User's consolidated SofaScore workbook
├── reports/                    # User-provided recruitment PDFs
└── utils/                      # Optional reusable data processing modules
```

## 6. Decisions still to validate

- The exact Excel file and schema must be available when implementation begins.
- The **5-3-2** is the chosen display shape, but actual most-used XI needs positional eligibility validation; actual historical formation frequency cannot be inferred from season totals alone.
- Verify whether age, yellow/red cards, goalkeeping data, player images, dorsals and positional detail are present.
- Choose the final percentile reference group and handling of small samples.
- Choose 2–3 recruitment candidates and supply their reports after squad/market analysis.
- Preserve the user's current app code and assets; this document is **not** a replacement `app.py`.

## 7. Change log

- **v1.0 (2026-10-21):** Captured approved landing page and agreed designs for Squad Analysis, Market Scouting (modal filters), Player Comparison (overview + full stats), and Recruitment Reports (2–3 PDF cards). Set data-integrity and implementation rules.
