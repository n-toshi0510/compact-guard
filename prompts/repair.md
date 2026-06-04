# Repair Prompt

compacted summary が `repair-needed` または `unsafe-to-resume` に classified されたときに使う prompt です。

目的は、利用可能な context から safer summary を作ることです。この prompt は facts、approvals、verification results、completed work、user intent を invent してはいけません。

## Prompt

compact-guard policy と rubric に従って compacted summary を repair してください。

compaction は実行しないでください。built-in compact / compress feature を置き換えないでください。Unsupported assumptions を使わないでください。あなたの task は、available context から safer repaired summary を作成することです。

### Inputs

#### Original Compacted Summary

```md
[PASTE ORIGINAL COMPACTED SUMMARY HERE]
```

#### Compact-Guard Evaluation

```md
[PASTE POST-COMPACT EVALUATION HERE]
```

#### Available Recovery Context

```md
[PASTE ANY AVAILABLE SESSION NOTES, PRESERVATION BRIEF, FILE STATUS, COMMAND RESULTS, OR USER CLARIFICATION HERE]
```

### Repair Requirements

利用可能な場合、次の must-preserve categories を restore または clarify してください。

- User Intent
- Current Task Status
- Constraints
- Decisions
- Unresolved Questions
- Risks
- Modified Files
- Commands Run
- Verification Results
- Pending Next Steps

Uncertainty は保持してください。category を recover できない場合は、`Unknown`、`Not established`、または `Requires clarification` と mark してください。

### Safety Rules

- Verification results を invent しない。
- Failed または skipped checks を passed checks に変えない。
- Evidence なしに task completion を claim しない。
- Constraints、non-goals、risks、blockers を削除しない。
- Unresolved questions を隠さない。
- Guess を fact に変えない。
- Original context が要求しない限り vendor-specific にしない。

### Output Format

次の形式で返してください。

```md
## Repaired Compacted Summary

### User Intent

### Current Task Status

### Constraints

### Decisions

### Unresolved Questions

### Risks

### Modified Files

### Commands Run

### Verification Results

### Pending Next Steps

## Repair Notes

### Restored Context

### Still Unknown

### Resume Recommendation
pass | repair-needed | unsafe-to-resume
```

Resume recommendation は compact-guard rubric に従ってください。Critical context がまだ missing の場合、`pass` を recommend しないでください。
