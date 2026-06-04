# Resume Prompt

`pass` に classified された compacted summary、または safe resume に十分な repaired summary から resume するときに使う prompt です。

この prompt を使って `repair-needed` または `unsafe-to-resume` findings を bypass しないでください。

## Prompt

あなたは compacted summary または repaired summary から AI coding session を resume します。

Summary を session continuity の現在の source として扱ってください。ただし、それを correctness の証明として扱わないでください。Preserved constraints、unresolved risks、verification evidence、pending next steps を尊重してください。

### Summary to Resume From

```md
[PASTE PASS OR REPAIRED SUMMARY HERE]
```

### Resume Rules

1. Changes を加える前に、current user intent と immediate next action を再 stated してください。
2. Preserved constraints と non-goals を尊重してください。
3. Unresolved questions と risks は silently resolve せず、見える状態で保持してください。
4. Modified files が mention されている場合、editing 前に file state を verify してください。
5. Summary に evidence が含まれていない限り、tests、builds、reviews、approvals、commands が passed したと claim しないでください。
6. Verification evidence が missing の場合、prior work に依存する前に何を verify すべきかを述べてください。
7. Summary が blockers または unsafe uncertainty を示す場合、stop して clarification または reconstruction を求めてください。
8. Preserved scope と pending next steps の範囲内だけで続行してください。

### Required Check Before Action

Implementation action の前に、次へ答えてください。

- Latest user intent は何か。
- 何が complete、in progress、blocked、deferred か。
- どの constraints と non-goals がまだ適用されるか。
- どの files または artifacts が affected になり得るか。
- どの verification evidence が存在するか。
- どの risks または unresolved questions が残っているか。
- Next safest action は何か。

### Output Format

次の形式で返してください。

```md
## Resume Readiness

### Intent

### State

### Constraints

### Evidence

### Risks and Open Questions

### Next Safest Action

### Stop Conditions
```

Summary が safe resume に十分でない場合、proceed しないでください。compact-guard rubric を使って issue を `repair-needed` または `unsafe-to-resume` に classify し、どの context が missing かを説明してください。
