# Post-Compact Evaluation Prompt

既存 compact / compress feature が compacted summary を生成した後に使う prompt です。

compact-guard は AI APIs、hosted services、vendor-specific evaluators を必要としません。この prompt は human reviewer または既存 agent workflow が compact-guard rubric を手動適用するためのものです。

## Prompt

以下の compacted summary を compact-guard summary evaluation rubric で評価してください。

まだ summary を repair しないでください。まず `pass`、`repair-needed`、`unsafe-to-resume` のいずれかに classify してください。

### Compacted Summary

```md
[PASTE COMPACTED SUMMARY HERE]
```

### Evaluation Dimensions

各 dimension を `present`、`partial`、`missing` のいずれかで mark してください。

- Intent
- Scope
- State
- Constraints
- Evidence
- Risks
- Next Actions

### Classification Rules

すべての critical dimensions が present で、critical contradiction がない場合のみ `pass` としてください。

少なくとも 1 つの critical dimension が partial で、critical contradiction がなく、欠落 detail を recover できる場合は `repair-needed` としてください。

次のいずれかに該当する場合は `unsafe-to-resume` としてください。

- Intent が missing または contradicted。
- Constraints が missing または contradicted。
- Active implementation state が missing。
- Evidence が invented または materially misleading。
- Safe resume risks または blockers が missing。
- Next actions が unsupported assumptions を必要とする。

### Required Context Omission Rules

- Missing user intent は通常 `unsafe-to-resume`。
- Missing current task status は、planning work では `repair-needed`、active implementation work では `unsafe-to-resume`。
- Missing constraints または non-goals は、それらが prohibited actions に関係する場合 `unsafe-to-resume`。
- Missing verification results は、recoverable なら `repair-needed`、evidence なしに completion を claim しているなら `unsafe-to-resume`。
- Missing unresolved risks は、recoverable なら `repair-needed`、risk が correctness、safety、data loss、user instructions に影響するなら `unsafe-to-resume`。

### Output Format

次の形式で返してください。

```md
## Compact-Guard Evaluation

### Outcome
pass | repair-needed | unsafe-to-resume

### Dimension Ratings
| Dimension | Rating | Evidence |
|-----------|--------|----------|
| Intent | present/partial/missing | |
| Scope | present/partial/missing | |
| State | present/partial/missing | |
| Constraints | present/partial/missing | |
| Evidence | present/partial/missing | |
| Risks | present/partial/missing | |
| Next Actions | present/partial/missing | |

### Critical Findings

### Missing or Ambiguous Context

### Resume Decision
Session が今 resume 可能か、先に repair が必要か、または reconstruction / clarification のために stop すべきかを書いてください。
```

Missing facts を invent しないでください。Evidence が支えない限り、summary を `pass` と mark しないでください。
