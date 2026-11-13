# Engineering Handoff Note

> Module 4 · Production Specs. Open the black box, make the build legible to an engineer.

## What this is

_One paragraph an engineer can read in 60 seconds._

Daymark is a React/Vite prototype for testing whether a focused Today view and low-friction activity logging can help sales reps manage follow-ups in a CRM instead of a shadow spreadsheet. src/app/DaymarkRouter.tsx owns client-side routes and auth gating; one shared workspace route keeps transient state alive while src/app/DaymarkWorkspace.tsx composes the screens. App navigation is in WorkspaceNavigation.tsx; follow-up and activity state/actions live in their feature hooks, and Supabase reads/writes stay behind features/prototype-state. Supabase Auth limits access to invited reps, with account-scoped RLS for deals, follow-ups, activities, and preferences. Sample deal content is fictional, pilot metrics are supplied research, and adoption is not measured. The automated flows use Supabase mocks; verify the live invited-user read/write flow separately before relying on it. See PRD.md for scope and evidence boundaries.

## Architecture (plain language)

- **Frontend:** React/Vite + Wouter; DaymarkRouter.tsx (routes + auth gating, one shared WorkspaceRoute), DaymarkWorkspace.tsx composes screens, WorkspaceNavigation.tsx; src/features/ (follow-ups, deals, activities, pilot-research, help, prototype-state, auth) with useFollowUpWorkspace / useActivityWorkspace hooks
- **Backend / data:** Supabase (direct from browser via supabase-js): Auth for one invited rep + tables deals, follow_ups, activities, user_preferences with owner-scoped RLS (auth.uid()); migration 0001_core_schema.sql applied; data adapter in features/prototype-state. api-server is unrelated.
- **Key flows:** 1. Sign in and load data: An invited rep signs in through Supabase Auth. The workspace reads account-scoped records; on an empty account it seeds sample deals and imports valid legacy browser state once.
2. Work a follow-up: Today filters, sorts, and paginates sample records; a rep can log an outcome in one tap or open deal details for context and optional notes.
3. Plan and review: My week groups follow-ups by day and supports rescheduling; Activity groups saved records and supports filtering, editing, deleting, and undo.
4. Explore evidence: Pilot & research separates supplied baselines, proposed targets, and unknown measures; its proposed kill switch is not an approved policy. Help contains the walkthrough video and shortcut reference.
5. Navigate: Sidebar links and the command palette change client-side routes. The shared workspace route is important for interactions that navigate and then focus a control, such as “Log activity for…”.

## What's solid vs. what's duct tape

| Area | State | Notes |
|---|---|---|
| Supabase auth/RLS, code ownership boundaries & test/build health | solid | Supabase access split into an Auth provider and a data adapter; owner-scoped RLS, no admin key in the browser; legacy local state imported only for an empty account; routing/navigation, feature state/actions and persistence have distinct owners; TypeScript checks pass; Playwright suite 46 passed, 11 intentional skips (mocked Supabase); production build succeeds (source-map and large-chunk warnings); prototype limits stated in UI and PRD. |
| Workspace coordinator, multi-table writes, one-rep auth, embedded content & CSS | rough | DaymarkWorkspace.tsx still coordinates cross-screen keyboard behavior and overlays; browser writes hit several tables without one transaction (partial failure can leave a partial save; retry exists but needs integration testing); auth is one-rep only (no sign-up, sharing, access UI or analytics); sample records and research content embedded in frontend code; feature CSS still in src/pages. |

## Risks & assumptions for the team

- The core hypothesis is NOT validated. The prototype has no pilot cohort, measured repeat-use data, or measured due-follow-up coverage.
- The displayed 11-minute, 18%, 8-field, 63%, and 2.1/5 figures are supplied research, not measurements from this app. Pilot scorecard thresholds and the stop rule are proposals pending an accountable owner.
- Automated authenticated-flow tests use a Supabase mock; there is no repeatable live-account or second-account RLS test. Verify the live invited-user read/write flow and RLS separately before sign-off.
- The first signed-in user must be created in Supabase Auth by the project owner. Public sign-up is intentionally unavailable.
- The legacy browser-state import is local to the browser that performs first initialization; other browsers do not have that local history to import.
- Only the configured owner account can see records. Adding team access requires a reviewed ownership model and RLS changes.
- Sample date/time behavior follows the browser’s local timezone. Validate timezone and daylight-saving behavior against the pilot’s required timezone before relying on due-time reporting.
- The route and core-flow tests cover in-app navigation and selected refresh flows. Confirm direct URL entry and refresh behavior in the eventual deployed environment, where the server must return the SPA entry point for client-side routes.
- The separate API service should not be treated as a Daymark backend; Supabase is the current data path.

## How to run it

```
Run commands from the monorepo root. In Replit, start the existing "artifacts/daymark: web" workflow; it supplies the artifact’s PORT and BASE_PATH.

Config: VITE_SUPABASE_URL and VITE_SUPABASE_PUBLISHABLE_KEY configure the browser client. SUPABASE_DB_URL is used only by the migration runner; never bundle it or a service-role key into the frontend.

Local dev (set both required variables):
PORT=5000 BASE_PATH=/ pnpm --filter @workspace/daymark run dev

Useful checks:
pnpm --filter @workspace/daymark typecheck
pnpm --filter @workspace/scripts typecheck:responsive
pnpm --filter @workspace/scripts test:responsive
PORT=5000 BASE_PATH=/ pnpm --filter @workspace/daymark build

Apply the database schema after SUPABASE_DB_URL contains the project’s Session pooler URI:
pnpm --filter @workspace/daymark run supabase:migrate

Then create one Auth user in the Supabase dashboard and sign in through Daymark. The responsive suite runs Playwright across the configured desktop and phone viewports; authenticated interactions still require an authorized test account. The production build writes to artifacts/daymark/dist/public.
```
