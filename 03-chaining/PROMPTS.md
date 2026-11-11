# PROMPTS.md: Living Prompt Pack

> Module 3 · Prompt Chaining. Re-architect the build with prompt chains; capture the reusable ones here.

## How to use this pack

_Each prompt is a reusable step. Chain them: the output of one becomes the input to the next._

## Prompt chain: Layout Hardening Chain

### Step 1: Expand: add the two missing screens
```
Add two focused screens to the existing Daymark sales-rep prototype. My week should let reps review completed, still-open, and upcoming follow-ups for the sample week, with deal context and amounts. Quick add should capture a follow-up using only a deal, one concise next action, and a due date, then make the saved item appear in Today and My week.

Add navigation for both screens on desktop and mobile, and keep the existing Daymark visual style. Preserve Today’s one-screen fit, filters, prominent deal amounts, visible next actions, pagination, and one-tap activity logging. Label sample data and device-local changes clearly; don’t imply CRM sync, external actions, or measured team adoption.
```

### Step 2: Behavior: handle loading, empty and error states
```
Apply the following logic constraints to the Activity History flow:
- Use skeleton screens for the List of all the tasks to be worked loading state.
- If no data is present, show the empty state: "No activity found".
- On fetch failure, trigger the error state: "Data load failed to complete".

Maintain the same design language throughout and tether all behavior strictly to these rules.
```

### Step 3: Refine: polish one thing on one screen
```
The Pilot and Research needs a professional Asana Style layout polish.
1. Start by listing the 3 biggest gaps in typography and spacing compared to Asana dashboard for experiment data.
2. Once you've identified those, resize the headers and update the complete layout to match.

Don't change anything else in the project or touch the underlying logic.
```

## Reusable techniques learned

- Split the chain into Expand, Behavior, and Refine prompts, with one clear goal for each. Carry the same hypothesis, metrics, quotes, and constraints through every prompt to prevent drift. Name the exact screen and limit each refinement prompt to one change. Check each step before continuing so the next prompt builds on verified results. When a behavior fails, add a focused correction prompt instead of restarting the whole chain.

## What broke (and the fix)

_Where a single mega-prompt failed and chaining fixed it._

What broke: Prompts that bundled screen changes, behavior rules, and visual polish made the scope easy to lose. Activity History also needed explicit loading, empty, and error states.

The fix: Split the chain into Expand, Behavior, and Refine. Carry the hypothesis and “don’t change other screens or logic” constraint forward, and verify each step before continuing.
