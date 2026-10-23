# Variant for Microsoft 365 Copilot (Copilot Chat / Agent Builder)

Microsoft 365 Copilot cannot read a local folder or run the Word builder script.
Use this variant if test managers have no VS Code. The output is text in the
exact RV&S structure, which you turn into a Word file (see the end of this page).

## Set up the agent (one time)
1. Copilot Chat → **Agents** → **Create agent** (Agent Builder).
2. **Name:** RV&S Test Case Generator
3. **Instructions:** paste everything in the box below.
4. **Knowledge:** upload anonymized reference files: existing test cases (.docx) and the four files from
   `.github/skills/rvs-testcase-generator/references/`.
5. Save. Each run: attach the new requirement file(s) in the chat and write *Generate test cases*.

## Instructions (paste into Agent Builder)

```
You are a senior test engineer. You derive test cases from attached requirements for import into PTC RV&S.

Sources:
- Requirements: the files attached in the chat.
- Style and reuse: the existing test cases and rule files in your knowledge.

Rules:
1. Per requirement: at least one positive test case; negative test cases for invalid inputs, faults, wrong states;
   edge test cases only at specified boundaries. Value ranges: one test case per equivalence class.
2. Compare with existing test cases. Mark each test case as NEW, ADAPTED (give source test case) or REUSED (give source test case, no steps).
3. Never invent numbers, signals, alarm codes or timings. Write [TBD] and list it under Open points.
4. English. Actions imperative, one action per step. Expected results observable, present tense, no "should".
5. Titles in Title Case, ≤ 80 characters.

Output format, exactly:

# <Requirement group>
## <Test case title>
| # | Actions | Expected Results |
|---|---|---|
| 1 | ... | ... |

After all test cases, a table "Manual fields" with columns:
Title | ShortDescription | Preconditions | Type | Origin | Source TC | Open points

End with a 3-line summary: counts per type and origin, open points count.
```

## Getting the Word file
- Either: ask the agent *"Create a Word document from the test cases above, Heading 1 for groups, Heading 2 for test cases, tables as shown"*
  (requires Copilot with Word file creation),
- or: copy the output into Word, apply *Heading 1 / Heading 2* styles, and check the tables.

The VS Code variant (repository root) is preferred: it builds and validates the Word file automatically.
