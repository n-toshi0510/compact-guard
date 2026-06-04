# Summary Evaluation Rubric

既存 compact / compress feature が compacted summary を生成した後、この rubric を使います。

compact-guard は AI APIs、hosted service、vendor-specific evaluator を必要としません。Human reviewer または既存 agent が、この rubric を手動で適用できます。

## Outcome Levels

### Pass

compacted summary は resume してよい品質です。

次を満たす場合に `pass` とします。

- ユーザーの最新 intent が明確である。
- Current task status が明確である。
- 重要な constraints と non-goals が保持されている。
- 関連 decisions が保持されている。
- 重要な modified files または artifacts が特定されている。
- Verification evidence が存在する、または not run と明示されている。
- Risks と unresolved questions が見えている。
- Next actions が concrete である。

### Repair-Needed

compacted summary はまだ resume するには十分安全ではありませんが、利用可能な context から repair できる可能性があります。

次の場合に `repair-needed` とします。

- 1 つ以上の required categories が incomplete だが recoverable である。
- Verification evidence が欠落しているが、commands または results を別 context から回収できる。
- Decisions が過度に compressed されているが、decision direction は回収できる。
- Next steps が vague だが、preserved state から再構築できる。
- Risks は言及されているが、resume を導くには specific ではない。

`repair-needed` summary からは、repair が完了するまで resume しないでください。

### Unsafe-to-Resume

compacted summary は critical context を欠落または歪曲しており、そのまま継続すると unsafe です。

次の場合に `unsafe-to-resume` とします。

- ユーザーの latest intent が missing または contradicted である。
- Essential constraints または explicit non-goals が失われている。
- Current implementation state を判断できない。
- Verification status が absent で、reconstruct もできない。
- Known risks または blockers が summary から消えている。
- Summary が facts、completed work、approvals、test results を invent している。
- Next actions が guessing を必要とする。

`unsafe-to-resume` summary は、source context からの reconstruction、user clarification、または safer repaired summary が必要です。

## Evaluation Dimensions

各 dimension を `present`、`partial`、`missing` で評価します。

| Dimension | Present | Partial | Missing |
|-----------|---------|---------|---------|
| Intent | Latest user goal and scope が明確 | Goal はあるが scope または latest instruction が曖昧 | Goal が absent または stale |
| Scope | In-scope と out-of-scope が見える | Boundary の片側だけが見える | Boundaries が absent |
| State | Completed、in-progress、blocked、deferred work が明確 | 一部 state は見えるが incomplete | Current state を判断できない |
| Constraints | Important instructions と non-goals が保持されている | Constraints が粗すぎる | Constraints が absent または contradicted |
| Evidence | Commands と verification results が保持、または not run と明示 | Evidence はあるが result detail が不足 | Verification status が absent |
| Risks | Known risks、blockers、unresolved questions が見える | Risks はあるが vague | Risks が消えている |
| Next Actions | Next steps が concrete かつ ordered | Next steps は方向性があるが incomplete | Next steps が guessing を必要とする |

## Classification Rules

すべての critical dimensions が present で、critical contradiction がない場合のみ `pass` とします。

少なくとも 1 つの critical dimension が partial で、critical contradiction がなく、欠落 detail を recover できる場合は `repair-needed` とします。

次のいずれかに該当する場合は `unsafe-to-resume` とします。

- Intent が missing または contradicted。
- Constraints が missing または contradicted。
- Active implementation work の state が missing。
- Evidence が invented または materially misleading。
- Safe resume に影響する risks または blockers が missing。
- Next actions が unsupported assumptions を必要とする。

## Required Context Omission Rules

compacted summary が user intent を omit している場合、latest intent が adjacent preserved context から即座に recover できる場合を除き、`unsafe-to-resume` とします。

compacted summary が current task status を omit している場合、planning work では `repair-needed`、active implementation work では `unsafe-to-resume` とします。

compacted summary が constraints または non-goals を omit している場合、それらが resumed agent の prohibited action に関係するなら `unsafe-to-resume` とします。

compacted summary が verification results を omit している場合、results を recover できるなら `repair-needed`、summary が evidence なしに completion を claim しているなら `unsafe-to-resume` とします。

compacted summary が unresolved risks を omit している場合、risk を recover できるなら `repair-needed`、risk が correctness、safety、data loss、user instructions に影響するなら `unsafe-to-resume` とします。

## Manual Evaluation Checklist

1. Repair せずに compacted summary を一度読む。
2. 各 evaluation dimension を `present`、`partial`、`missing` で mark する。
3. Invented facts、contradicted constraints、unsupported completion claims を確認する。
4. `pass`、`repair-needed`、`unsafe-to-resume` のいずれかを assign する。
5. Outcome が `pass` でない場合は、resume 前に repair するか clarification を求める。

## Boundary Reminder

この rubric は resume safety を評価します。Resumed agent の implementation choices の correctness を保証せず、review、tests、user approval を置き換えません。
