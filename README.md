# RV&S Test Case Generator for GitHub Copilot

Generates test cases for new requirements, using your existing test cases as reference, and builds a
Word file that the PTC RV&S Word importer reads directly.

```
input/requirements/        new requirements (.docx / .xlsx)          ─┐
input/existing_testcases/  reference test cases (.docx / .xlsx)      ─┼─►  Copilot  ─►  output/<date>_<Name>/
input/templates/           optional: rvs_template.docx (your styles) ─┘                   ├─ <Name>_RVS_import.docx
                                                                                          ├─ <Name>_manual_fields.xlsx
                                                                                          └─ testcases.json
```

- **RVS_import.docx** – one heading per test case, step table `# | Actions | Expected Results`. Import into RV&S.
- **manual_fields.xlsx** – ShortDescription, Preconditions, origin (NEW / ADAPTED / REUSED), source test case, open points.
  Preconditions are entered manually in RV&S; REUSED test cases are linked there with Reference Mode *Reuse*.
- **testcases.json** – the editable source. Change requests go here, then the Word file is rebuilt.

## One-time setup (per PC)

| Need | Check |
|---|---|
| VS Code with GitHub Copilot (Chat, Agent mode) | Copilot icon in VS Code |
| Python 3.10+ | `python --version` |
| Git | `git --version` |

1. Clone the repository:
   ```
   git clone <repo-url>
   ```
   Copies the repository from GitHub to your PC.
2. Open the folder in VS Code (*File → Open Folder*).
3. Install the Python packages:
   ```
   pip install -r requirements.txt
   ```
   Installs the three libraries the scripts need to read and write Word/Excel files (python-docx, openpyxl, PyYAML).
   If your company proxy blocks pip, ask IT to install these three packages.
4. If `/generate-testcases` or the skill does not appear in Copilot Chat, open VS Code *Settings*, search for **skills** and enable Agent Skills. Older VS Code versions may need an update.

## Daily use

1. Copy the new requirement files into `input/requirements/`.
2. Copy relevant existing test cases (RV&S Word export) into `input/existing_testcases/`.
3. Open Copilot Chat, switch to **Agent** mode, and type:
   ```
   /generate-testcases
   ```
   Or pick the agent **testcase-generator** in the agent dropdown and write: *Generate test cases for the new requirements.*
4. Copilot asks for a run name, extracts the files, writes the test cases and builds the Word file. Approve the terminal commands when asked.
5. Review the Word file and the `[TBD]` placeholders, then import into RV&S.

### More test cases or changes
```
/more-testcases
```
e.g. *Add 2 negative test cases for low oil pressure* or *Merge test cases 3 and 4*. Copilot edits the latest `testcases.json` and rebuilds the Word file.
Free questions work too: *"Which requirements have only positive test cases?"*

## Adapting it

| What | Where |
|---|---|
| Heading levels, table columns, grouping, Excel sheet on/off | `config/agent-config.yaml` |
| Coverage rules (positive / negative / edge, equivalence classes) | `.github/skills/rvs-testcase-generator/references/test-design-rules.md` |
| Wording rules | `.../references/writing-style.md` |
| House style by example | Put anonymized files into `examples/requirements/` and `examples/existing_testcases/` |
| Company Word styles | `input/templates/rvs_template.docx` |
| Signal names / tolerances for vTestStudio/CANoe later | `automation_hints: true` in the config |

## First RV&S import check
Import `examples/expected_output/GensetControl_Example_RVS_import.docx` into a **sandbox** document first.
Confirm that *Heading 1 → Heading* and *Heading 2 → Test Case* in the import mapping. If your mapping differs,
change `group_heading_level` / `testcase_heading_level` in the config.

## Confidentiality
`.gitignore` excludes everything in `input/` and `output/`. Only commit anonymized examples.
Keep the repository private.

## Microsoft 365 Copilot (without VS Code)
See `m365-copilot/agent-instructions.md`. Works with attached files in chat, but cannot build or validate the Word file automatically.

## Repository layout
```
.github/
  copilot-instructions.md               always-on rules for Copilot in this repo
  agents/testcase-generator.agent.md    custom agent (agent dropdown)
  prompts/generate-testcases.prompt.md  /generate-testcases
  prompts/more-testcases.prompt.md      /more-testcases
  skills/rvs-testcase-generator/
    SKILL.md                            workflow
    references/                         design rules, style, schema, RV&S format
    scripts/read_inputs.py              .docx/.xlsx → Markdown + test case library
    scripts/build_docx.py               testcases.json → validated Word + Excel
config/agent-config.yaml
examples/                               anonymized samples + expected output
input/  output/                         working folders (git-ignored)
m365-copilot/                           variant for Microsoft 365 Copilot
```
