# Code Review for Rubric Derivation

Read every file in the plugin before proposing any rubric. The SKILL.md defines the workflow; the reference files define the quality bar.

## Reading order

1. Main `SKILL.md` — workflow, phases, rules, quality bar.
2. Each file in `references/` — document-type-specific rules, banned patterns, evidence requirements.
3. Any agent briefs in `agents/` if present.

## What to extract from each file

### Explicit prohibitions
Words, phrases, or patterns the plugin bans. These become voice or humanness rubrics.
> Example: cogumi-writer bans "leverage," "robust," "seamless," "in conclusion." → Rubric: no banned word appears in the output.

### Quality claims
Statements of the form "every X must have Y" or "the output will Z." These become completeness or accuracy rubrics.
> Example: "Support every factual claim with a source, or label it as an assumption." → Rubric: every factual claim in the output is sourced or labeled.

### Workflow steps the plugin claims to follow
If the plugin says it asks clarifying questions, reviews an outline, or runs a self-check, the output should show evidence of each step. These become process rubrics.
> Example: cogumi-writer Step 5 says to run a conflict and number check. → Rubric: no number appears in two places with different values.

### Evidence requirements
"Never invent a statistic," "use only facts from the user's material." These become factual grounding rubrics.

### Format rules
Sentence length targets, heading style, paragraph length limits. These become structural rubrics.

### The self-check or quality bar section
Many plugins include a checklist or self-check step. Read it as the plugin's own definition of a passing output.

## What NOT to include in rubrics

- General writing quality beyond what the plugin claims (e.g., "is this engaging?" if the plugin does not mention engagement).
- Subjective preferences not stated in the plugin code.
- Failure modes you infer without textual evidence in the plugin.

The rubrics test the plugin against its own promises. Keep them narrow and grounded.

## Output of the code review

A list of candidate rubric dimensions with:
- The plugin rule or claim it tests.
- The file and section where the rule appears.
- A draft Pass/Fail definition.

Present 4–6 of these to the user for confirmation.
