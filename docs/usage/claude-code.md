# Claude Code Usage

この guide は、Claude Code の既存 `/compact [instructions]` workflow の前後に compact-guard を手動で適用する方法を示します。

compact-guard は Claude Code の built-in compact behavior を置き換えず、変更せず、実行もしません。Claude Code が compaction engine であり、compact-guard は policy、rubric、prompts、examples を提供する surrounding layer です。

Official reference:

- Claude Code commands reference: https://code.claude.com/docs/en/commands

## Assumption

この guide は、Claude Code session 内で `/compact` を使えることを前提にします。Claude Code docs は `/compact [instructions]` を、conversation so far を summarize して context を空ける command として説明し、optional focus instructions を渡せると説明しています。

## Workflow Placement

### 1. Apply the pre-compact prompt

Before running `/compact`, paste or adapt:

- [Pre-compact prompt](../../prompts/pre-compact.md)

目的:

- Must Preserve categories を compact 前に明示する。
- Claude Code の optional instructions に入れたい focus を整理する。
- compact-guard が compaction engine ではないことを維持する。

### 2. Run the existing built-in compact feature

Claude Code で既存 command を実行します。

```text
/compact
```

必要に応じて、Claude Code が対応する optional instructions に preservation focus を含めます。

```text
/compact Preserve latest user intent, current task state, constraints, verification evidence, unresolved risks, and concrete next actions.
```

これは compact-guard による automation ではありません。Claude Code の built-in command に、ユーザーが manual instruction を渡すだけです。

### 3. Apply the post-compact evaluation prompt

Compacted summary が得られたら、次を使って評価します。

- [Post-compact evaluation prompt](../../prompts/post-compact-evaluation.md)
- [Summary evaluation rubric](../rubrics/summary-evaluation.md)

Evaluation dimensions:

- Intent
- Scope
- State
- Constraints
- Evidence
- Risks
- Next Actions

### 4. Apply the repair prompt if needed

Evaluation outcome が `repair-needed` または `unsafe-to-resume` の場合、resume する前に次を使います。

- [Repair prompt](../../prompts/repair.md)
- [Bad summary example](../../examples/bad-summary.md)
- [Repair walkthrough example](../../examples/repair-walkthrough.md)

Repair では、unsupported assumptions を fact に変えないでください。Claude Code が compact した summary であっても、verification evidence がない completion claim は evidence として扱いません。

### 5. Apply the resume prompt

Evaluation outcome が `pass`、または repaired summary が safe resume に十分になったら、次を使います。

- [Resume prompt](../../prompts/resume.md)

Resume prompt は、preserved constraints、unresolved risks、verification evidence、pending next steps を action 前に再確認させます。

## Do Not

- compact-guard が `/compact` を置き換えると説明しない。
- compact-guard が Claude Code behavior を変更すると説明しない。
- Hosted evaluator、AI API integration、package scripts、CLI automation を要求しない。
- Optional instructions を渡しただけで compacted summary が safe resume に十分になったと assume しない。

## Quick Checklist

```text
Pre-compact prompt
→ Claude Code /compact [optional focus instructions]
→ Post-compact evaluation prompt
→ Repair prompt if needed
→ Resume prompt
```
