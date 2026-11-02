---
name: ab
description: >
  Quick A/B eval: baseline vs one named plugin or skill. Auto-detects
  whether the target is a plugin or a skill, skips arm-selection, and
  runs all four phases (design, generate, judge, report). Use when you
  want a fast two-arm comparison without configuring a full eval.
---

# A/B Eval

Runs a two-arm comparison: **baseline** vs **one named target** (plugin or skill).

This is a streamlined path through the full eval workflow. All four phases run, but arm selection is automatic and the confirmation step is shortened.

---

## Entry

The user invokes `/cogumi-eval:ab <target>` where `<target>` is a plugin name (e.g. `cogumi-writer`) or a skill name (e.g. `cogumi-writer:write`).

If no target is given, ask for one before proceeding.

**Detect target type:**
- If `<target>` contains `:`, treat it as a skill (e.g. `cogumi-writer:write` → read `skills/cogumi-writer/write.md`).
- Otherwise, attempt to locate `skills/<target>/SKILL.md`. If found, treat as a skill. If not found, treat as a plugin and read its plugin manifest.

Set arms: `[baseline, <target>]`. Do not offer arm editing.

---

## Phase 1: Eval Design (abbreviated)

Follow Steps 2–6 of `SKILL.md` exactly, with one change:

**Skip Step 1** (arm identification — already set above).

Present the confirmation message with arms pre-filled and locked. The user may still edit rubric dimensions, scenarios, and persona.

Do not proceed to Phase 2 until the user confirms.

---

## Phase 2: Generation

Follow Phase 2 of `SKILL.md` exactly.

- Baseline arm: read `references/baseline.md`.
- Target arm: if skill, read `references/plugin-executor.md` and pass the skill's SKILL.md and reference files. If plugin, same — pass the plugin's full skill directory.

---

## Phase 3: Judging

Follow Phase 3 of `SKILL.md` exactly.

---

## Phase 4: Report

Follow Phase 4 of `SKILL.md` exactly.

The report header must note this was a quick A/B run (two arms only).
