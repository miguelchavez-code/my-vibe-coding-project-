# Engineering Handoff Note

> Module 4 · Production Specs. Open the black box, make the build legible to an engineer.

## What this is

_One paragraph an engineer can read in 60 seconds._

Daymark is a React/Vite prototype for testing whether a focused Today view and low-friction activity logging can help sales reps manage follow-ups in a CRM instead of a shadow spreadsheet. Wouter handles client-side routes; a shared workspace shell coordinates navigation, keyboard shortcuts, dialogs, and state, while feature folders hold the Today/Week, deal detail, Activity, Pilot & research, and Help screens. Sample deals are generated in the browser, and activities, follow-up edits, and the private spreadsheet-reliance check-in are saved in that browser’s localStorage. There is no Daymark API integration, authentication, shared database, analytics pipeline, or real CRM/email/calling action. The interface interactions are functional, but adoption and success metrics are not measured; see PRD.md for the product scope and evidence boundaries.

## Architecture (plain language)

- **Frontend:** React/Vite + Wouter routes (/today, /week, /activity, /deals/:id, /research, /help) via one shared WorkspaceRoute; src/pages/Today.tsx coordinates state; screens grouped in src/features/ (follow-ups, deals, activities, pilot-research, help, prototype-state)
- **Backend / data:** No backend: sample deals generated in-browser (createSampleDeals); activities, follow-up edits and check-in saved to localStorage key followup-prototype-v1 (validated on load); research content is static. artifacts/api-server exists but is NOT connected.
- **Key flows:** 1. Work a follow-up: Today filters, sorts, and paginates sample records; a rep can log an outcome in one tap or open deal details for context and optional notes.
2. Plan and review: My week groups follow-ups by day and supports rescheduling; Activity groups locally saved records and supports filtering, editing, deleting, and undo.
3. Explore evidence: Pilot & research separates supplied baselines, proposed targets, and unknown measures; its proposed kill switch is not an approved policy. Help contains the walkthrough video and shortcut reference.
4. Navigate: Sidebar links and the command palette change client-side routes. The shared workspace route is important for interactions that navigate and then focus a control, such as “Log activity for…”.

## What's solid vs. what's duct tape

| Area | State | Notes |
|---|---|---|
| Routes, follow-up flows, feature structure & local data validation | solid | Main route and follow-up flows covered by responsive browser tests (31 passed, 11 configured skips); clearer boundaries for types, sample data, persistence and screens; saved activity validated with explicit loading/empty/error states; prototype limits stated in UI and PRD; TypeScript checks and production build pass (non-blocking Vite sourcemap warnings). |
| Workspace state, persistence, data source & styling | rough | Today.tsx is still a large coordinator not yet split into hooks/services; localStorage is single-browser only (no sync, backup, ownership or cohort reporting); sample records and research content embedded in frontend code; Activity loading/error simulates local reads, not API latency/outages/retry; feature CSS still in the older src/pages stylesheet area. |

## Risks & assumptions for the team

- The core hypothesis is NOT validated. The prototype has no pilot cohort, measured repeat-use data, or measured due-follow-up coverage.
- The displayed 11-minute, 18%, 8-field, 63%, and 2.1/5 figures are supplied research, not measurements from this app. Pilot scorecard thresholds and the stop rule are proposals pending an accountable owner.
- Local storage can be cleared, blocked, or become unreadable, and its data cannot be combined across people or devices. Do not use it for real customer records.
- Sample date/time behavior follows the browser’s local timezone. Validate timezone and daylight-saving behavior against the pilot’s required timezone before relying on due-time reporting.
- The app has been tested for in-app route transitions. Confirm direct URL entry and refresh behavior in the eventual deployed environment, where the server must return the SPA entry point for client-side routes.
- The separate API service should not be treated as a Daymark backend unless an explicit integration and data contract are added.

## How to run it

```
Run commands from the monorepo root. In Replit, start the existing "artifacts/daymark: web" workflow; it supplies the artifact’s PORT and BASE_PATH.

Local dev (set both required variables):
PORT=5000 BASE_PATH=/ pnpm --filter @workspace/daymark run dev

Useful checks:
pnpm --filter @workspace/daymark typecheck
pnpm --filter @workspace/scripts typecheck:responsive
pnpm --filter @workspace/scripts test:responsive
PORT=5000 BASE_PATH=/ pnpm --filter @workspace/daymark build

The responsive suite runs Playwright across the configured desktop and phone viewports. The production build writes to artifacts/daymark/dist/public.
```
