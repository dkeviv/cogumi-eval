# Report Format

Publish one HTML artifact after all scenarios are judged. The report must be self-contained — readable by someone who was not present in this conversation.

## Required sections

### 1. Header

- Plugin name and version (from plugin.json if available).
- Eval mode: A/B or eval-only.
- Date and model used as judge.
- Number of scenarios run vs. attempted.

### 2. Rubric scorecard

A compact table: rubric dimensions as rows, outputs as columns (Plugin / Baseline in A/B, Plugin only in eval-only). Each cell shows Pass ✓ or Fail ✗. Include a totals row (e.g., "3/5 Pass").

This is the first thing a reader sees. Make it scannable.

### 3. Per-scenario results

For each scenario:

**A/B mode:**
- Scenario description (topic, format, constraint — one line each).
- Outputs side by side (or stacked on mobile).
- Below each output: the judge's critique for each dimension.

**Eval-only mode:**
- Scenario description.
- Output with dimension scores listed after it.
- Judge critiques per dimension.

### 4. Summary verdict

One paragraph: does the plugin achieve its stated quality bar overall? State the pass rate per dimension across all scenarios (e.g., "Humanness: 2/4 scenarios passed"). Call out the strongest and weakest dimensions by name.

Do not hedge. If the plugin fails on 3 of 5 dimensions, say so directly.

### 5. Recommendations

One bullet per failing dimension. Each bullet must:
- Name the specific plugin rule or section that is insufficient.
- Describe a concrete change (add a rule, remove a prohibition, add a self-check step).
- Tie the change to the judge's evidence from at least one scenario.

Do not recommend vague improvements like "improve voice" or "be more specific." Name what to change and where.

## Design

- Both light and dark theme.
- Scorecard is the hero element — visible without scrolling on desktop.
- Outputs side by side at ≥768px, stacked on mobile.
- Pass cells: green tint. Fail cells: red tint. Neutral background everywhere else.
- Judge critiques are collapsed by default per scenario; expand on click.
- Font: a readable serif for output text, sans-serif for labels and scores.
