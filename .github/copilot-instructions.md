# Repository instructions for GitHub Copilot

This repository generates test cases from requirements and exports them for import into PTC RV&S.

- For any request to create, derive, adapt or extend test cases, use the skill
  `.github/skills/rvs-testcase-generator/SKILL.md`.
- Input lives in `input/` (requirements, existing test cases, optional Word template). Never modify it.
- Output goes to `output/<YYYY-MM-DD_HHMM>_<Name>/`. The Word file is built only by
  `scripts/build_docx.py` from `testcases.json` – never written by hand.
- Configuration: `config/agent-config.yaml`.
- Test cases are written in English.
- The users are test managers, not developers: explain every terminal command in one sentence before running it,
  and keep reports short.
- Requirements and test cases are confidential. Do not send their content to web searches or external services.
