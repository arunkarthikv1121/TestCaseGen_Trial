---
description: Generate RV&S test cases for all requirements in input/requirements using existing test cases as reference
mode: agent
agent: agent
---

Generate test cases for the requirements in `input/requirements/`.

Use the skill `.github/skills/rvs-testcase-generator/SKILL.md` and follow all six workflow steps:
extract inputs → analyse against existing test cases → write `testcases.json` → build and validate the Word file → report.

Run name: ${input:runName:Short name for this run, e.g. BatteryMonitoring}

Settings come from `config/agent-config.yaml`. Explain each terminal command before running it.
