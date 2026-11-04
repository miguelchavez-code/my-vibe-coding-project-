# Prototype v2: Two Upgrade Passes (Lab 2)

> Module 1 · Velocity. Same prototype, two surgical upgrade passes, closing the gap from toy to tool.

## Weakest section (from v1)

_Where does it look like a toy? Where would a VP of Design refuse to open it? Where does the logic feel fake? Pick the one spot that costs you the most credibility._

The unexplained churn ranking. It claims to know which accounts will churn within 30 days,

## Upgrade paths run (pick two)

- [x] Design Match
- [x] Add Interactivity
- [x] Surgical Refinement
- [ ] Existing Product Track

## v2 build

- **Shareable link:** https://churn-radar-miguelchavezlop.replit.app
- **Before → after:** Before: purple-gradient dashboard → After: clean Google-style interface, with sorting, filters, account details, and transparent sample-data explanations preserved.
- **What each pass changed:**
  - Design Match: I changed the layout to stripe look and feel but once it is merged with the existent controls and colors it did not give a good result. I reverted back to my original Google layout.
  - Add Interactivity: Make the at-risk accounts list filterable by risk level. When I click an account, open a side panel showing that account's health score, its top churn reason, and its recent activity.
  - Surgical Refinement: "Focus only on making the churn ranking transparent and credible. The screen currently implies it can predict which accounts will churn within 30 days, but it uses sample data without a validated prediction model.

Change “Ranked by churn likelihood” to a “Sort by” dropdown beside the account-list heading. Offer “Lowest health score first” as the default and “Highest health score first” as the alternative. Both options must actually reorder the list. Replace predictive wording with “Accounts needing attention.” Describe the 30-day window as a review horizon, not a forecast. Move the “Sample data—not a churn forecast” disclosure directly above the list, alongside the sorting control. In the existing account side panel, explain the account’s position using its sample health score, active sorting rule, and existing risk reason. Clearly distinguish sample evidence from suggested actions; do not invent confidence levels or scoring formulas. Preserve the current styling, layout, risk filters, summary navigation, and recent activity. Do not add unrelated features.

Done means: users can immediately understand why an account appears where it does, change the ordering, and distinguish illustrative risk signals from genuine predictions."

## Show & Swap read, round 2

_A NEW partner, a blind read. What landed differently from v1?_

- **Feels like a real product, or a mockup?** Still feels like a mock-up from the user perspective still is not showing a one-screen.
- **Where interactivity fell short:** The scrolling experience is not nice and a graph would help, The prioritization is not emphasized and a graph could help to improve the experience.
- **Would they show it to a VP?** No, I agree it requires additional elements to help understand the user and layout presentation, revert back to a less colorful interface.
