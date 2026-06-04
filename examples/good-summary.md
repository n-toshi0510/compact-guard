# Good Compacted Summary Example

この example は、tool-neutral な AI coding session の acceptable compacted summary 例です。特定 vendor、model、CLI の挙動には依存しません。

この example 内の task state は synthetic session snapshot です。現在の repository status を表すものではありません。

## Good Summary

```md
## User Intent

ユーザーは compact-guard の documentation-first MVP を作っている。現在の request は、prompt pack や usage docs ではなく、policy と rubric に基づく examples を作ること。

## Current Task Status

Completed:
- Documentation package foundation を作成した。
- Non-goals document を作成した。
- Compaction policy を作成した。
- Summary evaluation rubric を作成した。
- Prompt pack を作成した。

In progress:
- Tool-neutral examples を作成中。

Deferred:
- Tool-specific usage docs はまだ作成しない。
- Runtime code、package scripts、AI API integrations、hosted services、CLI automation は追加しない。

## Constraints

- Documentation-first MVP に留める。
- compact-guard は compaction engine ではなく policy / rubric layer として説明する。
- Examples は tool-neutral にする。
- Facts、verification results、completed work、user intent を invent しない。

## Decisions

- Policy categories は Must Preserve、May Summarize、May Drop を使う。
- Rubric outcomes は pass、repair-needed、unsafe-to-resume を使う。
- Examples は bad summary、good summary、repair walkthrough に分ける。

## Unresolved Questions

- Tool-specific usage docs で各 CLI workflow のどこに prompts を差し込むかは未実装のため、まだ具体化していない。

## Risks

- Examples が具体性を欠くと rubric の使い方が伝わらない。
- Examples が tool-specific になりすぎると model-agnostic / vendor-neutral boundary が弱くなる。
- Repaired summary が事実を invent すると compact-guard の safety model と矛盾する。

## Modified Files

Created or planned in current examples slice:
- examples/bad-summary.md
- examples/good-summary.md
- examples/repair-walkthrough.md

Existing source-of-truth docs:
- docs/policies/compaction-policy.md
- docs/rubrics/summary-evaluation.md
- prompts/repair.md

## Commands Run

No validation commands recorded in this example.

## Verification Results

Verification status is not established in this example. Any real implementation should record actual checks separately.

## Pending Next Steps

1. Create bad summary example showing missing intent, state, evidence, risks, and vague next actions.
2. Create good summary example showing pass-level coverage.
3. Create repair walkthrough that restores recoverable context and leaves unknowns visible.
4. Keep usage docs deferred until the examples are complete.
```

## Rubric Classification

**Outcome:** `pass`

この summary は、resume に必要な critical dimensions を明示し、verification が未確立であることも unsupported claim にせず保持しています。

## Dimension Ratings

| Dimension | Rating | Evidence |
|-----------|--------|----------|
| Intent | present | Current request と scope が明示されている。 |
| Scope | present | Examples は in-scope、usage docs と runtime work は deferred / out-of-scope。 |
| State | present | Completed、in progress、deferred が分かれている。 |
| Constraints | present | Documentation-first、tool-neutral、no runtime、no invention が明示されている。 |
| Evidence | present | Verification status は not established と明示され、passed claim を invent していない。 |
| Risks | present | Concrete risks が列挙されている。 |
| Next Actions | present | Ordered next steps がある。 |

## Satisfied Criteria

### Policy Criteria

- **User Intent**: 最新 request と scope を保持している。
- **Current Task Status**: completed / in progress / deferred を保持している。
- **Constraints**: non-goals と implementation limits を保持している。
- **Decisions**: terminology と artifact split を保持している。
- **Unresolved Questions**: usage docs の未具体化を保持している。
- **Risks**: examples quality、tool-neutrality、fact invention risk を保持している。
- **Modified Files**: examples slice で関係する files を保持している。
- **Commands Run**: commands が記録されていないことを明示している。
- **Verification Results**: verification status が not established と明示されている。
- **Pending Next Steps**: ordered actions がある。

### Rubric Criteria

- `pass` に必要な critical dimensions が present。
- Evidence を invent していない。
- Next actions が guessing を必要としない。
- Known risks が summary から消えていない。

## Why This Is Safe Enough to Resume

この summary は、resumed agent が次に何を作るべきか、何を作ってはいけないか、どの uncertainty を保持すべきかを判断できます。

重要なのは、verification がまだ確立されていない点を正直に残していることです。`pass` は「すべてが正しい」ではなく、「resume に必要な context が十分に見える」ことを意味します。
