# Test design rules

## 1. Coverage per requirement
For every requirement (or requirement chapter, if no IDs exist):

| Type | When | Minimum |
|---|---|---|
| **positive** | Always | 1 test case proving the specified behaviour under nominal conditions |
| **negative** | When the requirement implies invalid input, fault, out-of-range value, wrong state or forbidden transition | 1 per distinct invalid class / fault reaction |
| **edge** | Only when it adds information: boundaries of value ranges, timing limits, state transitions at the limit, first/last item | 1 per relevant boundary |

Do not pad. A requirement without ranges or fault reactions may need only one positive test case.

## 2. Value ranges → equivalence classes
For every parameter with a range or threshold:

1. List classes: *below valid*, *valid* (split further if the requirement defines sub-ranges), *above valid*.
2. One test case (or one step block) per class with a representative value from the middle of the class.
3. Boundaries (min, max, threshold ± resolution) are **edge** test cases – only if the requirement gives a resolution or tolerance, or the boundary behaviour is specified.
4. Write the chosen values and the class into the step, e.g. `Set coolant temperature to 95 °C (valid class 80–110 °C).`
5. If a limit is missing: use `[TBD]` and add an `open_points` entry. Never invent numbers.

## 3. Reuse before writing (origin)
Compare each requirement with the existing test case library (`output/_work/tc_library.json` and the extracted Markdown).

| Origin | Criterion | Output |
|---|---|---|
| **REUSED** | An existing test case covers the requirement completely without change | Listed in the manual-fields sheet with `source_tc`; **not** written to the Word import (link it in RV&S with Reference Mode *Reuse*) |
| **ADAPTED** | An existing test case fits in structure but values, signals, states or expected results differ | Copy its structure and wording, change only what the new requirement demands; set `source_tc` |
| **NEW** | Nothing comparable exists | Write in the same style as the existing test cases |

When in doubt between REUSED and ADAPTED, choose ADAPTED.

## 4. Structure of one test case
- One test case = one objective. Title says *what* is verified under *which condition*.
- Step 1 normally establishes or checks the starting state that is not already in the preconditions.
- One action per step. The expected result describes what is **observable** (display, signal value, state, log entry, timing).
- Last step verifies the requirement's core statement explicitly.
- Return-to-normal steps (e.g. *Restore nominal voltage*) only if the requirement specifies recovery behaviour.
- 2–15 steps (see `config/agent-config.yaml`). Split longer ones.

## 5. Preconditions
Short, testable state statements, mirroring the style of the existing test cases, e.g.
`Plant ready for operations.` · `No alarms.` · `Engine is not running.`
They go to the manual-fields sheet, not into the Word table.

## 6. Automation readiness (only if `automation_hints: true`)
Append signal names and tolerances in square brackets where the source gives them:
`Set supply voltage to 21.9 V [Sig: PS_Voltage_Set, tol ±0.1 V].`
