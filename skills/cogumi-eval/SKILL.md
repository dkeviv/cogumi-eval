---
name: cogumi-eval
description: >
  Evaluate any Claude plugin or skill. Reads its code to derive rubrics,
  confirms the eval design with the user, runs the plugin through a dynamic
  simulated user interaction, judges results against a baseline, and publishes
  a report. Use when asked to evaluate, test, or benchmark a plugin or skill.
---

# Plugin Eval

Evaluates a Claude plugin or skill end-to-end. Reads the plugin code to derive rubrics, runs it through a realistic simulated user, and judges results.

## Modes

- **A/B** — same scenario runs with and without the plugin; judge compares both outputs.
- **Eval-only** — plugin output only; judge scores against rubrics with no baseline comparison.

---

## Phase 1: Eval Design

### Step 1. Identify the target

Ask which plugin or skill to evaluate. The user provides a name, path, or repository URL. If a path, confirm it exists before proceeding.

### Step 2. Read the plugin code

Read `references/code-review.md` first, then follow its checklist against every file in the plugin's `skills/` directory and `references/` folder. Do not skip any file — reference files often contain the quality bar the plugin actually enforces.

### Step 3. Derive rubric dimensions

From the code review, extract 4–6 rubric dimensions. Each dimension maps one plugin claim or rule to a binary test.

Format:

| Dimension | What it tests | Pass | Fail |
|---|---|---|---|
| Name | Which plugin rule | Output meets the rule | Output violates the rule |

Derive only rubrics grounded in the plugin's own code. Do not add general writing quality rubrics the plugin does not claim.

### Step 4. Suggest test scenarios

Based on the plugin's stated document types and use cases, propose 3–5 scenarios. Each scenario contains:

- **Topic** — what the output is about.
- **Goal** — what the reader should know, decide, or do.
- **Format** — document type the plugin supports (blog post, executive brief, email, etc.).
- **Constraint** — one specific challenge that stresses the rubrics (a technical topic that tempts jargon, a sensitive topic that tempts hedging, a data-heavy topic that tempts vague estimates).

### Step 5. Propose a user persona

Propose one user persona for the proxy subagent. The persona must:
- Have a realistic role and domain context.
- Know what they want but not how to achieve it perfectly.
- Have opinions — able to redirect or push back when output misses the mark.
- Match the scenarios' domain.

Example: "You are the head of engineering at a 60-person SaaS company. You want a thought leadership post for the company blog about switching from weekly all-hands to async updates. You have strong opinions about meetings and will push back on any draft that sounds like it could apply to any company."

### Step 6. Confirm with user

Present in one structured message:
1. Rubric dimensions (table, editable).
2. Test scenarios (list, editable — add, remove, or rewrite any).
3. User persona (editable).
4. Mode choice: A/B or eval-only.
5. Number of scenarios to run (default: all proposed).

**Do not proceed to Phase 2 until the user explicitly confirms or edits and confirms.**

---

## Phase 2: Generation

For each confirmed scenario, spawn the following subagents. Run scenarios in parallel where the session allows.

### Plugin executor subagent

Read `references/plugin-executor.md` for the full subagent brief.

Pass to the subagent:
- The plugin's full SKILL.md and all its reference files (as instructions, not just context).
- The confirmed user persona.
- The scenario.

The subagent runs the plugin's workflow start to finish, acting as the user proxy when the plugin requests input. It returns only the final output document.

### Baseline subagent (A/B mode only)

Read `references/baseline.md` for the full subagent brief.

Pass to the subagent:
- The user persona.
- The scenario.
- No plugin instructions.

The subagent writes the output as a capable writer with no structured guidance. It returns only the final output document.

---

## Phase 3: Judging

Read `references/judge.md` for judge prompt construction and output schema.

Spawn one judge subagent per scenario. Pass:
- The confirmed rubric dimensions with Pass/Fail definitions.
- The plugin executor output.
- The baseline output (A/B mode only).

The judge returns a structured verdict for every dimension. Do not accept a verdict that lacks a direct quote from the output as evidence.

---

## Phase 4: Report

Read `references/report.md` for the report structure.

Compile all scenario verdicts and publish one HTML artifact. The report must be self-contained — readable by someone who was not in this conversation.

---

## Failure handling

- If the plugin executor cannot complete the plugin workflow (missing information, ambiguous instructions), log the incomplete scenario and continue with the remaining ones. Note it in the report.
- If fewer than 2 scenarios complete successfully, do not publish a report. Tell the user what blocked them.
- If the user rejects the rubrics in Phase 1 more than twice without converging, ask what specific quality they want to measure and rewrite from there.
