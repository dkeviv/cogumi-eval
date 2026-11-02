# Report Format

Publish one HTML artifact after all scenarios are judged. The report must be self-contained — readable by someone who was not present in this conversation.

## Required sections

### 1. Header

- Plugin(s) under evaluation and their versions (from plugin.json if available).
- Arms list: baseline + each plugin arm name.
- Date and model used as judge.
- Number of scenarios run vs. attempted.

### 2. Rubric scorecard

A compact table: rubric dimensions as rows, arms as columns. Each cell shows Pass ✓, Fail ✗, or Incomplete —. Include a totals row (e.g., "3/5 Pass").

This is the first thing a reader sees. Make it scannable. Number of columns = number of arms.

### 3. Per-scenario results

For each scenario:

- Scenario description (topic, format, constraint — one line each).
- Arm outputs displayed in columns (one column per arm). At ≥768px show columns side by side; on mobile stack them.
- Below each output: the judge's critique for each dimension, collapsed by default, expand on click.

### 4. Summary verdict

One paragraph per arm: does this arm achieve the rubrics' quality bar overall? State the pass rate per dimension across all scenarios (e.g., "Humanness: 2/4 scenarios passed"). Call out the strongest and weakest dimensions by name.

Do not hedge. If an arm fails 3 of 5 dimensions, say so directly.

Then one cross-arm conclusion: which arm performed best overall and on which dimensions. Name the arms explicitly.

### 5. Recommendations

One bullet per failing dimension, per arm that failed it. Each bullet must:
- Name the arm.
- Name the specific plugin rule or section that is insufficient (or note that the baseline has no rules to fix).
- Describe a concrete change (add a rule, remove a prohibition, add a self-check step).
- Tie the change to the judge's evidence from at least one scenario.

Do not recommend vague improvements like "improve voice" or "be more specific." Name what to change and where.

## Design

- Both light and dark theme.
- Scorecard is the hero element — visible without scrolling on desktop.
- Arm columns displayed side by side at ≥768px, stacked on mobile.
- Pass cells: green tint. Fail cells: red tint. Incomplete cells: grey tint. Neutral background everywhere else.
- Judge critiques are collapsed by default per scenario; expand on click.
- Font: a readable serif for output text, sans-serif for labels and scores.
- With 3+ arms, use a horizontal scroll container on the output columns so narrow viewports stay usable.
