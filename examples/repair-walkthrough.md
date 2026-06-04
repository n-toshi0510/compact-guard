# Repair Walkthrough Example

この walkthrough は、bad compacted summary を safer repaired summary に変える方法を示します。

この walkthrough 内の task state は synthetic session snapshot です。現在の repository status を表すものではありません。

compact-guard は compaction を実行せず、facts を invent しません。Repair は、available recovery context から recover できる information を戻し、recover できない information を unknown として保持します。

## Starting Point: Bad Summary

```md
ユーザーは docs を作りたいと言っていた。いくつか Markdown を追加した。

たぶん次は残りの docs を続ければよい。テストは問題なさそう。
```

## Compact-Guard Evaluation

**Outcome:** `unsafe-to-resume`

| Dimension | Rating | Reason |
|-----------|--------|--------|
| Intent | missing | Latest user goal と exact scope がない。 |
| Scope | missing | In-scope / out-of-scope がない。 |
| State | missing | Completed / in-progress / deferred がない。 |
| Constraints | missing | No runtime、no AI API、tool-neutral などの constraints がない。 |
| Evidence | missing | 「テストは問題なさそう」は unsupported claim。 |
| Risks | missing | Unresolved risks がない。 |
| Next Actions | partial | 「残りの docs」が vague。 |

## Available Recovery Context

```md
User request:
- Implement examples only.
- Create examples/bad-summary.md, examples/good-summary.md, examples/repair-walkthrough.md.
- Use existing policy, rubric, and prompt pack as source of truth.
- Keep examples tool-neutral.
- Do not create tool-specific usage docs yet.
- Do not add runtime code, package scripts, AI API integrations, hosted services, or CLI automation.

Known completed docs before this slice:
- README.md
- LICENSE
- CONTRIBUTING.md
- docs/index.md
- docs/non-goals.md
- docs/policies/compaction-policy.md
- docs/rubrics/summary-evaluation.md
- prompts/pre-compact.md
- prompts/post-compact-evaluation.md
- prompts/repair.md
- prompts/resume.md

Required demonstration points:
- missing user intent
- missing current task state
- missing verification evidence
- missing unresolved risks
- vague next actions
- repaired summary that does not invent facts

Verification:
- No validation commands have been run in this recovery context.
```

## Repair Actions

### 1. Restore User Intent

Recovered from user request:

- Create three tool-neutral examples.
- Use policy, rubric, and prompt pack as source of truth.
- Do not implement usage docs or runtime work.

### 2. Restore Current Task State

Recovered from known completed docs:

- Foundation docs, policy, rubric, and prompt pack already exist.
- Examples are the current slice.
- Usage docs are deferred.

### 3. Restore Constraints

Recovered from user request:

- Documentation-first.
- Tool-neutral examples.
- No runtime code.
- No package scripts.
- No AI API integrations.
- No hosted services.
- No CLI automation.

### 4. Restore Verification Evidence Without Inventing Facts

The bad summary said, "テストは問題なさそう." That is not valid evidence.

Repair action:

- Remove the unsupported pass-like claim.
- Replace it with: `Verification status: No validation commands recorded in available recovery context.`

### 5. Restore Unresolved Risks

Recovered from task intent and rubric:

- Examples may be too abstract to teach rubric usage.
- Repaired summary may accidentally invent facts.
- Tool-neutral examples may not prepare users for tool-specific usage docs.

### 6. Make Next Actions Concrete

Recovered from user request:

1. Create bad summary example.
2. Create good summary example.
3. Create repair walkthrough.
4. Keep usage docs deferred.

## Repaired Compacted Summary

```md
## User Intent

ユーザーは compact-guard の examples slice のみを実装したい。作成対象は examples/bad-summary.md、examples/good-summary.md、examples/repair-walkthrough.md。Policy、rubric、prompt pack を source of truth とし、examples は tool-neutral にする。

## Current Task Status

Completed before this slice:
- README.md
- LICENSE
- CONTRIBUTING.md
- docs/index.md
- docs/non-goals.md
- docs/policies/compaction-policy.md
- docs/rubrics/summary-evaluation.md
- prompts/pre-compact.md
- prompts/post-compact-evaluation.md
- prompts/repair.md
- prompts/resume.md

In progress:
- examples/bad-summary.md
- examples/good-summary.md
- examples/repair-walkthrough.md

Deferred:
- Tool-specific usage docs.

## Constraints

- Do not create tool-specific usage docs yet.
- Do not add runtime code.
- Do not add package scripts.
- Do not add AI API integrations.
- Do not add hosted services.
- Do not add CLI automation.
- Keep examples tool-neutral.
- Do not invent facts.

## Decisions

- Bad example demonstrates missing intent, missing state, missing evidence, missing risks, and vague next actions.
- Good example demonstrates pass-level coverage without claiming unverified checks passed.
- Repair walkthrough demonstrates restoration from available recovery context and preserves unknowns.

## Unresolved Questions

- Tool-specific usage docs are not implemented yet, so exact CLI placement remains deferred.

## Risks

- Examples may be too abstract unless annotations map directly to rubric dimensions.
- Repair may overstate recovered context if uncertainty is not preserved.
- Verification evidence must not be invented.

## Modified Files

Expected files for this slice:
- examples/bad-summary.md
- examples/good-summary.md
- examples/repair-walkthrough.md

## Commands Run

Unknown from the bad summary. Available recovery context records no validation commands.

## Verification Results

Verification status: No validation commands recorded in available recovery context. Do not claim tests passed.

## Pending Next Steps

1. Create the bad summary example with rubric annotations.
2. Create the good summary example with satisfied criteria.
3. Create the repair walkthrough showing restored context and remaining unknowns.
4. Leave usage docs for a later task.
```

## Repair Notes

### Restored Context

- User intent was restored from the explicit user request.
- Current task state was restored from known completed docs and current requested files.
- Constraints were restored from the user request and non-goal boundary.
- Risks were restored from rubric criteria and task-specific failure modes.
- Next actions were made concrete from the requested files.

### Still Unknown

- Exact validation commands and results are unknown unless separately recorded.
- Any tool-specific workflow placement remains deferred.
- This repaired summary does not prove implementation correctness.

### Resume Recommendation

**Outcome:** `pass`

Reason: The repaired summary now preserves intent, scope, state, constraints, risks, modified files, and concrete next actions. Verification evidence is not invented; it is explicitly marked as not recorded in the available recovery context.

## What This Demonstrates

- Missing user intent can be restored only when explicit user request context is available.
- Missing current task state should be reconstructed from known completed artifacts and current requested files.
- Missing verification evidence must not be replaced with a fake pass.
- Missing unresolved risks should be restored as visible risk statements.
- Vague next actions should become concrete ordered actions.
- Unknowns must remain visible instead of being silently resolved.
