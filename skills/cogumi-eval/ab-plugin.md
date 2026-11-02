---
name: ab-plugin
description: >
  Run a single plugin arm in isolation: execute the named plugin
  end-to-end against a scenario and return its labelled output. No
  baseline. No comparison. No judging. Use to generate one arm's output
  for inspection, debugging, or to supply to a later judge manually.
---

# Plugin Arm Runner

Executes one plugin arm in isolation and returns its labelled output.

No baseline is run. No judging or report is produced. This command is for:
- Inspecting what a plugin actually outputs before committing to a full eval.
- Debugging a plugin arm that failed in a previous run.
- Generating an arm output to paste into a manual comparison.

---

## Entry

The user invokes `/cogumi-eval:ab-plugin <plugin-name>` where `<plugin-name>` is a plugin (e.g. `cogumi-writer`). Its skills directory must exist at `skills/<plugin-name>/`.

If no target is given, ask for one.

---

## Setup

Ask the user for:
1. **Scenario** — topic, goal, format, and one constraint. (One sentence each is fine.)
2. **User persona** — who the simulated user is. (Two sentences minimum: role, domain, and one strong opinion.)

If the user supplies both upfront, confirm them and proceed immediately. Do not ask for anything else.

---

## Execution

Read `references/plugin-executor.md` for the full subagent brief.

Spawn one subagent. Pass:
- The plugin's full SKILL.md and all files in its `references/` folder — as instructions.
- The user persona.
- The scenario.
- Arm name: `<plugin-name>`.

The subagent runs the plugin workflow end-to-end, acting as the user proxy at every input step.

---

## Output

Return the subagent's output exactly as received, preceded by:

```
ARM: <plugin-name>
SCENARIO: <topic> — <format>
```

Then the document.

Do not add commentary, critique, or scores. If the subagent reported an error or incomplete run, show that error verbatim and tell the user which step failed.
