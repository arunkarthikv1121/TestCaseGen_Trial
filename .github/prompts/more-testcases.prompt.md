---
description: Add or change test cases in the latest run and rebuild the RV&S Word file
mode: agent
agent: agent
---

Open the most recent `output/*/testcases.json` (newest folder name).

Change request: ${input:request:e.g. "Add 2 negative test cases for low oil pressure" or "Split test case X"}

Rules:
- Follow `.github/skills/rvs-testcase-generator/references/test-design-rules.md` and `writing-style.md`.
- Keep all other test cases unchanged.
- Rebuild with `python .github/skills/rvs-testcase-generator/scripts/build_docx.py <run_folder>` and fix any validation error.
- Report only what changed and the output file path.
