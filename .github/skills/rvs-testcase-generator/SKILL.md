---
name: rvs-testcase-generator
description: Generate test cases for new requirements based on existing test cases and export them as a Word file importable into PTC RV&S (Integrity). Use when asked to create, derive, generate, adapt or add test cases, test specifications or test steps from requirements in the input folder, or to produce an RV&S import file.
---

# RV&S Test Case Generator

Creates test cases for the requirements in `input/requirements/`, reusing the style and content of the test cases in `input/existing_testcases/` (and `examples/`), and builds a Word file for the RV&S importer.

Read these before generating – they are the rules, not suggestions:
- [Test design rules](references/test-design-rules.md) – coverage, equivalence classes, REUSED / ADAPTED / NEW
- [Writing style](references/writing-style.md) – titles, actions, expected results
- [testcases.json schema](references/testcases-schema.md) – the format you write
- [RV&S Word format](references/rvs-word-format.md) – what the output looks like and why

Settings: `config/agent-config.yaml`.
Gold example of a finished run (input → `testcases.json` → Word file): `examples/expected_output/`.

## Workflow

### 1. Prepare
Check Python and packages once per machine (explain each command to the user before running it):
```
python --version
pip install -r requirements.txt
```

### 2. Extract the inputs
```
python .github/skills/rvs-testcase-generator/scripts/read_inputs.py
```
This converts every .docx/.xlsx into Markdown under `output/_work/` and parses existing test cases into `output/_work/tc_library.json`.
If it reports no requirement files, stop and tell the user to put files into `input/requirements/`.

### 3. Analyse
Read all files in `output/_work/requirements/`, `output/_work/existing_testcases/`, `output/_work/examples/` and `tc_library.json`.
For each requirement (ID if present, else chapter heading):
1. Identify behaviour, parameters with ranges, states, fault reactions, timings.
2. Find the closest existing test cases (same function, same signals, similar wording).
3. Decide REUSED / ADAPTED / NEW per the design rules.
4. Plan positive, negative and – where sensible – edge test cases; derive equivalence classes for every value range.

### 4. Write testcases.json
Create `output/<YYYY-MM-DD_HHMM>_<Name>/testcases.json` following the schema.
- Copy wording patterns, precondition style and step granularity from the existing test cases.
- Missing values → `[TBD]` + an `open_points` entry. Never invent limits, signals, timings or alarm numbers.

### 5. Build and validate
```
python .github/skills/rvs-testcase-generator/scripts/build_docx.py output/<run_folder>
```
If validation fails, fix `testcases.json` and rerun until it passes. Do not edit the .docx manually.

### 6. Report to the user (in chat, short)
- Output folder and file names
- Count per requirement: positive / negative / edge, and NEW / ADAPTED / REUSED
- REUSED test cases with their source IDs (to be linked in RV&S)
- Requirements with `[TBD]` or open points
- Requirements you could not cover and why

## Follow-up requests
For "add more test cases for …", "add negative tests", "change step 3 of …":
open the latest `output/*/testcases.json`, edit it, rerun step 5. Keep existing titles unchanged unless asked.

## Hard rules
- Requirements and test cases are confidential: never paste them into web searches or external tools.
- Output only into `output/`. Never modify files in `input/` or `examples/`.
- One heading per test case, step table with exactly the columns from the config.
