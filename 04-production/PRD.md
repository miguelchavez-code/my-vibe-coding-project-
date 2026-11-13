# Living PRD

> Module 4 · Production Specs. Refactor for readability; extract a living PRD that stays true as the build evolves.

## Problem

_What user problem does this solve? Tie to the validated hypothesis._

Sales reps who currently track follow-ups outside the CRM face a cluttered home, unclear next steps, and high-friction activity logging. They may keep a shadow spreadsheet because it is faster to see what to do and record what happened there.

The product hypothesis under test is: Replacing the cluttered CRM home with a focused view of today’s follow-ups and one-tap activity logging will cause reps who track follow-ups outside the CRM to manage those follow-ups in the CRM instead of a separate spreadsheet.

This is a working hypothesis, not a validated pilot result. The prototype displays supplied research baselines and sample CRM records; it does not measure repeated use or follow-up coverage across a sales-rep cohort.

## Users & jobs

- **Primary user:** Sales reps, including account executives, who currently manage some or all follow-ups in a separate spreadsheet.
- **Job to be done:** During a busy selling day, quickly identify the next action for each due deal, log the result with minimal effort, and retrieve recent activity without maintaining a separate tracker.

## Scope

- **In:** - A Today view of due, overdue, upcoming, and logged follow-ups with visible next actions, deal values, stage filters, search, sorting, and pagination.
- One-tap activity logging from a follow-up, with optional outcome, note, and next-follow-up date.
- Deal details, a weekly follow-up view, and an Activity history view.
- Pilot & research content that distinguishes supplied research, proposed targets, unknown baselines, and measured results.
- Help content with the product walkthrough and keyboard shortcuts.
- **Out (explicitly):** - Connecting to a production CRM, email, calendar, or calling system.
- Authentication, shared team records, or server-backed persistence.
- Collecting analytics or calculating cohort-level adoption and follow-up coverage.
- Claiming that the product hypothesis or proposed pilot thresholds have been validated.

## Requirements

| # | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| 1 | Show the rep’s work for today | Must (P0) | Today presents due and overdue follow-ups with company, contact, concise next action, due time, stage, and deal value. Search, stage/value filters, sorting, and pagination keep records reachable on desktop and mobile; the list fits the normal desktop viewport and remains accessible with scrolling at short heights or high zoom. |
| 2 | Log an activity with low friction | Must (P0) | A rep can log a standard outcome directly from a Today card in one action. Optional outcome, note, and next-follow-up date are available without requiring a note or date. |
| 3 | Create a follow-up without leaving the work view | Should (P1) | The global New follow-up action and C shortcut open the same form. A rep can add a next action and due date to a sample or newly named company, with the record saved locally. |
| 4 | Preserve follow-up and activity context | Must (P0) | A deal detail view shows the next action, deal facts, prior context, and recent activity. The rep can edit the next action and due date separately from logging an activity. |
| 5 | Review and manage the week | Should (P1) | My week groups follow-ups by day, shows completion and overdue counts, and lets the rep open, log, or reschedule a follow-up. |
| 6 | Find and correct logged activity | Should (P1) | Activity groups records by date, filters by outcome, and supports editing or deleting an entry. Loading, empty, and local-data read-failure states are distinguishable; retry is available after a read failure. |
| 7 | Keep navigation and shared actions reachable | Should (P1) | Today, My week, Activity, deal details, Pilot & research, and Help are available through their client-side routes. The global command palette and keyboard shortcuts remain available. |
| 8 | Separate research evidence from pilot decisions | Should (P1) | Pilot & research shows supplied baselines and quotes as supplied research, labels targets as proposals, labels unknown baselines as needing measurement, and does not present sample-session counts as team adoption. |
| 9 | Explain the prototype’s limits | Should (P1) | The UI and PRD state that deal records are samples, activity is saved on the current device, and the prototype does not place calls or send messages. |
| 10 | Provide a product walkthrough | Could (P2) | Help includes the overview video, getting-started guidance, and a keyboard-shortcut reference. |

## Data & events

_What gets stored, what gets tracked._

- Deals, contacts, stages, values, and follow-up actions: seeded sample records in the browser; example contact details use non-routable .example addresses and sample phone numbers. [MOCKED]
- Activity outcome, note, timestamp, and next date: created and edited through the prototype; stored in browser localStorage on this device. [REAL interaction, local-only persistence]
- Follow-up edits and completion state: stored with the prototype’s local state in the same browser. [REAL interaction, local-only persistence]
- Activity loading, empty, and error states: loading is a brief local read state; empty means no matching locally saved activity; error represents unreadable or invalid local saved data, not a failed API request. [Implemented locally; no network fetch]
- User spreadsheet-reliance check-in: a self-reported selection saved locally on this device. [REAL interaction, not cohort research]
- Research quotes and baseline metrics: supplied content shown on Pilot & research: 11 minutes versus a 2-minute target, 18% weekly active adoption, 8 required fields, 63% shadow-spreadsheet reliance, and 2.1/5 internal CSAT. [SUPPLIED research, not prototype telemetry]
- Pilot outcomes and proposed thresholds: repeat use, due-follow-up logging, spreadsheet reliance, and deal-update time are shown as targets or unmeasured results. [PROPOSED or unknown; no pilot results collected]
- Product analytics: no events are sent to an analytics service; the local activity count is not team adoption or pilot coverage. [NOT implemented]
- External communication: no email or call is initiated by the prototype. [NOT implemented]

## Open questions

- Who owns the pilot decision and approves the proposed continue, revise, stop, or inconclusive rules?
- Which sales-rep cohort will participate, and how will exposure to the prototype be confirmed?
- What are the cohort’s measured pre-pilot baselines for repeat use and due-follow-up coverage?
- How should weekly active use, eligible due follow-ups, and spreadsheet reliance be defined and measured?
- What data source and retention policy will be used if the pilot moves beyond this local prototype?
- Should the private spreadsheet-reliance check-in remain anonymous, and how should its responses be used?
- Are the proposed numerical scorecard thresholds accepted by the pilot owner, or should they be revised before launch?
