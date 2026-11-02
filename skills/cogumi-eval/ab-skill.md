---
name: ab-skill
description: >
  Run a single skill arm in isolation: execute the named skill
  end-to-end against a scenario and return its labelled output. No
  baseline. No comparison. No judging. Use to generate one arm's output
  for inspection, debugging, or to supply to a later judge manually.
---

# Skill Arm Runner

Executes one skill arm in isolation and returns its labelled output.

No baseline is run. No judging or report is produced. This command is for:
- Inspecting what a skill actually outputs before committing to a full eval.
- Debugging a skill arm that failed in a previous run.
- Generating an arm output to paste into a manual comparison.

---

## Entry

The user invokes `/cogumi-eval:ab-skill <skill-ref>` where `<skill-ref>` is either:
- `<plugin>:<skill>` (e.g. `cogumi-writer:write`) — reads `skills/<plugin>/<skill>.md`.
- `<plugin>` alone (e.g. `cogumi-writer`) — reads `skills/<plugin>/SKILL.md`.

If the file does not exist at the resolved path, tell the user and stop.

If no target is given, ask for one.

---

## Setup

Ask the user for:
1. **Scenario** — topic, goal, format, and one constraint. (One sentence each is fine.)
2. **User persona** — who the simulated user is. (Two sentences minimum: role, domain, and one strong opinion.)

If the user supplies both upfront, confirm them and proceed immediately.

---

## Execution

Read `references/plugin-executor.md` for the full subagent brief.

Spawn one subagent. Pass:
- The resolved skill file (SKILL.md or `<skill>.md`) and all files in its `references/` folder — as instructions.
- The user persona.
- The scenario.
- Arm name: `<skill-ref>` (use the full ref as given, e.g. `cogumi-writer:write`).

The subagent runs the skill workflow end-to-end, acting as the user proxy at every input step.

---

## Output

Return the subagent's output exactly as received, preceded by:

```
ARM: <skill-ref>
SCENARIO: <topic> — <format>
```

Then the document.

Do not add commentary, critique, or scores. If the subagent reported an error or incomplete run, show that error verbatim and tell the user which step failed.
