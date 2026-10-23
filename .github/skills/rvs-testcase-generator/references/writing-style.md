# Writing style

Mirror the style of the existing test cases first. Where they give no guidance, use these rules.

## Titles (Text heading)
- Title Case, noun phrase, ≤ 80 characters, no trailing period.
- Pattern: `<Function / Object> <Behaviour> <Condition>`
  - `Genset System Availability after Power Cycle`
  - `Low Battery Voltage Warning Below Threshold`
  - `Start Request Rejected During Active Shutdown Alarm`
- Do not number titles; RV&S numbers sections.

## ShortDescription
≤ 60 characters, sentence case, no period: `Operational system after boot up`.

## Actions
- Imperative, present tense, one action: `Switch off the main supply.` `Press the START button on the HMI.`
- Name the interface exactly as in the requirement (HMI, CAN signal, push button, parameter).
- Include values and units with a space: `24.0 V`, `500 ms`, `95 °C`.

## Expected results
- Present tense, observable, verifiable: `The engine starts within 10 s.` `Alarm A102 'Low oil pressure' is displayed.`
- No *should*, *may*, *correctly*, *as expected*, *properly*.
- One expected result per action. If nothing observable happens, state the observable state: `No alarm is displayed.`

## Terminology
- English. Keep German product terms or signal names unchanged if the source uses them.
- Use the same names for components, alarms and states as the requirement – never synonyms.
