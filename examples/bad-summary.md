# Bad Compacted Summary Example

この example は、tool-neutral な AI coding session の compacted summary 例です。意図的に不十分な summary を示し、compact-guard rubric でなぜ unsafe かを annotated します。

この example 内の task state は synthetic session snapshot です。現在の repository status を表すものではありません。

## Bad Summary

```md
ユーザーは docs を作りたいと言っていた。いくつか Markdown を追加した。

たぶん次は残りの docs を続ければよい。テストは問題なさそう。
```

## Rubric Classification

**Outcome:** `unsafe-to-resume`

この summary は active documentation work の continuation に必要な critical context を失っています。特に user intent、current task state、verification evidence、unresolved risks、next actions が欠落または曖昧です。

## Dimension Ratings

| Dimension | Rating | Evidence |
|-----------|--------|----------|
| Intent | missing | 「docs を作りたい」だけで、latest user goal、scope、explicit instruction がない。 |
| Scope | missing | どの docs が in-scope / out-of-scope か不明。 |
| State | missing | どの files が created / edited / pending / deferred か不明。 |
| Constraints | missing | non-goals、vendor-neutral、runtime code 禁止などの constraints が消えている。 |
| Evidence | missing | 「テストは問題なさそう」は command、result、not run のどれでもなく unsupported claim。 |
| Risks | missing | unresolved risks や blockers が存在するか判断できない。 |
| Next Actions | partial | 「残りの docs」が vague で、次に何を作るべきか判断できない。 |

## Violations

### Missing User Intent

Policy の **User Intent** category に違反しています。

問題:

- 最新 user goal がない。
- 実装対象 task が不明。
- 「docs」という broad label だけで、どの artifact を作るべきか判断できない。

Rubric impact:

- Intent が missing。
- Required Context Omission Rules により、latest intent が recover できない場合は `unsafe-to-resume`。

### Missing Current Task State

Policy の **Current Task Status** と **Modified Files** categories に違反しています。

問題:

- 完了済み work が不明。
- In-progress / blocked / deferred work が不明。
- 作成済み files が不明。

Rubric impact:

- State が missing。
- Active implementation work では current task status の omission は `unsafe-to-resume`。

### Missing Verification Evidence

Policy の **Commands Run** と **Verification Results** categories に違反しています。

問題:

- 「テストは問題なさそう」が evidence なしの completion-like claim になっている。
- Commands run がない。
- Checks が passed / failed / skipped / not run のどれか不明。

Rubric impact:

- Evidence が missing。
- Evidence なしの completion claim は `unsafe-to-resume`。

### Missing Unresolved Risks

Policy の **Risks** と **Unresolved Questions** categories に違反しています。

問題:

- Known risks があるか不明。
- Unresolved questions が見えない。
- Constraints や non-goals の omission が risk として残っていない。

Rubric impact:

- Risks が missing。
- Safe resume に影響する risks / blockers が missing の場合は `unsafe-to-resume`。

### Vague Next Actions

Policy の **Pending Next Steps** category に違反しています。

問題:

- 「残りの docs」が concrete ではない。
- Task order、dependencies、stop conditions がない。

Rubric impact:

- Next Actions が partial。
- Unsupported assumptions を必要とする next actions は `unsafe-to-resume` に近づく。

## Why This Must Not Be Used for Resume

この summary から resume すると、resumed agent は次を guess する必要があります。

- ユーザーが今どの docs を求めているか。
- どの files が既に作成されたか。
- どの constraints を守るべきか。
- 何が verified されたか。
- どの risks を保持すべきか。

compact-guard rubric では、これらの guess が必要な状態を safe resume として扱いません。
