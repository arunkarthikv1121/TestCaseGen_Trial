# testcases.json – the intermediate format

Copilot writes this file; `scripts/build_docx.py` turns it into the RV&S Word file.
Never write the .docx by hand – always go through this file so the layout stays importable.

```json
{
  "name": "BatteryMonitoring",
  "generated_from": ["input/requirements/REQ_Battery_Monitoring.docx"],
  "test_cases": [
    {
      "group": "Battery Voltage Monitoring",
      "title": "Low Battery Voltage Warning Below Threshold",
      "short_description": "Warning raised when battery voltage drops below lower limit",
      "type": "negative",
      "origin": "ADAPTED",
      "source_tc": "5130899",
      "requirement_refs": ["Battery Voltage Monitoring"],
      "preconditions": [
        "System powered and in operational mode.",
        "No active alarms."
      ],
      "steps": [
        {"action": "Set supply voltage to 24.0 V.", "expected": "No battery warning is displayed."},
        {"action": "Reduce supply voltage to 21.9 V.", "expected": "Warning 'Low battery voltage' is displayed within [TBD] s."}
      ],
      "open_points": ["Reaction time for warning not specified in requirement."]
    }
  ]
}
```

| Field | Required | Rules |
|---|---|---|
| `name` | yes | Short PascalCase name, used for output file names |
| `group` | no | Level-1 heading in Word (feature / requirement chapter). Default: first `requirement_refs` entry |
| `title` | yes | Becomes the test case heading = RV&S *Text* heading. Unique, ≤ 80 chars |
| `short_description` | yes | RV&S *ShortDescription*. ≤ 60 chars. Goes to the manual-fields sheet |
| `type` | yes | `positive` \| `negative` \| `edge` |
| `origin` | yes | `NEW` \| `ADAPTED` \| `REUSED` |
| `source_tc` | for ADAPTED/REUSED | RV&S ID or title of the existing test case |
| `requirement_refs` | yes | Requirement ID if present, else requirement heading or file name |
| `preconditions` | yes | List of short sentences. Goes to the manual-fields sheet (entered manually in RV&S) |
| `steps` | yes (not for REUSED) | `action` + `expected`, one action per step |
| `open_points` | no | Ambiguities, missing values. Goes to the manual-fields sheet |
