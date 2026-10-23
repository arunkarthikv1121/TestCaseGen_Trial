# RV&S Word import format

Target: PTC RV&S (Integrity) document import from Word, Test Case item type.

## Layout produced by `build_docx.py`

```
Heading 1   Battery Voltage Monitoring              → RV&S Heading item (section 4)
Heading 2   Low Battery Voltage Warning ...         → RV&S Test Case item, Text field heading (section 4.1)
            ┌───┬──────────────────┬─────────────────────┐
            │ # │ Actions          │ Expected Results    │  ← header row, bold, grey
            ├───┼──────────────────┼─────────────────────┤
            │ 1 │ ...              │ ...                 │
            └───┴──────────────────┴─────────────────────┘
Heading 2   Next test case ...
```

- Each test case is separated by its own heading. The heading text plus the table form the *Text* field.
- Preconditions and ShortDescription are **not** in the Word file. They are entered manually in RV&S,
  taken from `<name>_manual_fields.xlsx`.
- REUSED test cases are not in the Word file (they already exist in RV&S; link them with Reference Mode *Reuse*).
- Heading levels, column names, table style and grouping are set in `config/agent-config.yaml → rvs_word_import`.

## RV&S fields (from the project's document view)
| RV&S column | Source |
|---|---|
| Section | Assigned by RV&S from heading order |
| Category | Import mapping: Heading 1 → Heading, Heading 2 → Test Case |
| State | RV&S default (New) |
| Reference Mode | Add for imported items; Reuse for REUSED items (manual) |
| Preconditions | Manual, from manual-fields sheet |
| ShortDescription | Manual, from manual-fields sheet |
| Text | Heading + step table |
| ID / Document | Assigned by RV&S |

## Company styles
Place your company Word template as `input/templates/rvs_template.docx`.
The builder then uses its heading and table styles. Its content is ignored.

## Import check
Before the first productive use, import one generated file into a sandbox document in RV&S and confirm
the heading-to-category mapping. Adjust `group_heading_level` / `testcase_heading_level` if needed.
