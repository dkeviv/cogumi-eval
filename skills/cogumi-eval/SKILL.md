---
name: cogumi-eval
description: >
  Evaluate any Claude plugin or skill. Reads its code to derive rubrics,
  confirms the eval design with the user, runs each arm through a dynamic
  simulated user interaction, judges results across all arms, and publishes
  a report. Use when asked to evaluate, test, or benchmark a plugin or skill.
---

# Cogumi Eval

Evaluates Claude plugins or skills end-to-end. Reads plugin code to derive rubrics, runs each arm through a realistic simulated user, and judges results across all arms.

## Arms

An **arm** is one variant run per scenario. Every eval has at least two arms:

- **Baseline arm** — no plugin. A capable writer with no structured guidance. Always present.
- **Plugin arm(s)** — one or more plugins/skills under test. Each runs independently.

Examples:
- Single plugin: `[baseline, cogumi-writer]`
- Head-to-head: `[baseline, cogumi-writer, humanize-writing]`
- Version comparison: `[baseline, cogumi-writer-v1, cogumi-writer-v2]`

The baseline arm is always included. You cannot eval with only one arm total.

---

## Phase 1: Eval Design

### Step 1. Identify the arms

Ask which plugin(s) or skill(s) to evaluate. The user names one or more. For each, confirm the path or name resolves before proceeding.

If the user does not specify arms, default to: `[baseline, <the named plugin>]`.

### Step 2. Read each plugin's code

Read `references/code-review.md` first. Then follow its checklist against every file in each plugin's `skills/` directory and `references/` folder. Do not skip any file. Read all plugins before deriving rubrics.

### Step 3. Derive rubric dimensions

From the code review of all plugin arms, extract 4–6 rubric dimensions. Each dimension maps one quality claim or rule (from any arm) to a binary test. A dimension grounded in one plugin's rules still applies to all arms — that is what the comparison tests.

Format:

| Dimension | What it tests | Pass | Fail |
|---|---|---|---|
| Name | Which rule or quality claim | Output meets the rule | Output violates the rule |

Derive only rubrics grounded in at least one arm's code. Do not add general writing quality rubrics no arm claims.

### Step 4. Suggest test scenarios

Based on the document types and use cases supported across all arms, propose 3–5 scenarios. Each scenario contains:

- **Topic** — what the output is about.
- **Goal** — what the reader should know, decide, or do.
- **Format** — document type supported by the plugins (blog post, executive brief, email, etc.).
- **Constraint** — one specific challenge that stresses the rubrics (a technical topic that tempts jargon, a sensitive topic that tempts hedging, a data-heavy topic that tempts vague estimates).

### Step 5. Propose a user persona

Propose one user persona shared across all arms. The persona must:
- Have a realistic role and domain context.
- Know what they want but not how to achieve it perfectly.
- Have opinions — able to redirect or push back when output misses the mark.
- Match the scenarios' domain.

Example: "You are the head of engineering at a 60-person SaaS company. You want a thought leadership post for the company blog about switching from weekly all-hands to async updates. You have strong opinions about meetings and will push back on any draft that sounds like it could apply to any company."

### Step 6. Confirm with user

Present in one structured message:
1. Arms list (editable — add or remove plugin arms; baseline is always included).
2. Rubric dimensions (table, editable).
3. Test scenarios (list, editable — add, remove, or rewrite any).
4. User persona (editable).
5. Number of scenarios to run (default: all proposed).

**Do not proceed to Phase 2 until the user explicitly confirms or edits and confirms.**

---

## Phase 2: Generation

For each confirmed scenario, spawn one subagent per arm. Run all arms for a scenario in parallel where the session allows. Run scenarios sequentially unless parallelism is available.

### Baseline arm subagent

Read `references/baseline.md` for the full subagent brief.

Pass to the subagent:
- The user persona.
- The scenario.
- No plugin instructions.

### Plugin arm subagent(s)

Read `references/plugin-executor.md` for the full subagent brief.

For each plugin arm, spawn a separate subagent. Pass to each:
- That arm's full SKILL.md and all its reference files (as instructions, not just context).
- The confirmed user persona.
- The scenario.
- The arm name (so the subagent labels its output correctly).

Each subagent runs its plugin's workflow start to finish, acting as the user proxy when the plugin requests input. It returns only the final output document, labelled with its arm name.

---

## Phase 3: Judging

Read `references/judge.md` for judge prompt construction and output schema.

Spawn one judge subagent per scenario. Pass:
- The confirmed rubric dimensions with Pass/Fail definitions.
- All arm outputs for that scenario, each labelled by arm name.

The judge returns a structured verdict for every dimension across every arm. Do not accept a verdict that lacks a direct quote from the output as evidence.

---

## Phase 4: Report

Read `references/report.md` for the report structure.

Compile all scenario verdicts and publish one HTML artifact. The report must be self-contained — readable by someone who was not in this conversation.

---

## Failure handling

- If any arm's subagent cannot complete its workflow (missing information, ambiguous instructions), log that arm as incomplete for that scenario. Continue with the remaining arms and scenarios.
- If fewer than 2 arms complete at least 1 scenario, do not publish a report. Tell the user what blocked them.
- If the user rejects the rubrics in Phase 1 more than twice without converging, ask what specific quality they want to measure and rewrite from there.
