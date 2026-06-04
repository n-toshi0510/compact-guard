# Codex CLI Usage

この guide は、Codex CLI の既存 `/compact` workflow の前後に compact-guard を手動で適用する方法を示します。

compact-guard は Codex CLI の built-in compact behavior を置き換えず、変更せず、実行もしません。Codex CLI が compaction engine であり、compact-guard は policy、rubric、prompts、examples を提供する surrounding layer です。

Official reference:

- OpenAI Codex CLI slash commands: https://developers.openai.com/codex/cli/slash-commands

## Assumption

この guide は、Codex CLI session 内で `/compact` を使えることを前提にします。OpenAI docs は `/compact` を、長い会話を summarize して tokens を空ける command として説明しています。

## Workflow Placement

### 1. Apply the pre-compact prompt

Before running `/compact`, paste or adapt:

- [Pre-compact prompt](../../prompts/pre-compact.md)

目的:

- User Intent、Current Task Status、Constraints、Decisions、Unresolved Questions、Risks、Modified Files、Commands Run、Verification Results、Pending Next Steps を整理する。
- Codex が後続 compaction で保持すべき context を明確にする。
- compact-guard が compaction engine ではないことを維持する。

### 2. Run the existing built-in compact feature

Codex CLI で既存 command を実行します。

```text
/compact
```

Codex が confirmation を求める場合は、Codex CLI の UI に従ってください。

compact-guard はこの step の内部 behavior を所有しません。

### 3. Apply the post-compact evaluation prompt

Compacted summary が得られたら、次を使って評価します。

- [Post-compact evaluation prompt](../../prompts/post-compact-evaluation.md)
- [Summary evaluation rubric](../rubrics/summary-evaluation.md)

評価結果は次のいずれかにします。

- `pass`
- `repair-needed`
- `unsafe-to-resume`

### 4. Apply the repair prompt if needed

Evaluation outcome が `repair-needed` または `unsafe-to-resume` の場合、resume する前に次を使います。

- [Repair prompt](../../prompts/repair.md)
- [Repair walkthrough example](../../examples/repair-walkthrough.md)

Repair では、verification results、completed work、approvals、user intent を invent しないでください。回収できない context は `Unknown`、`Not established`、または `Requires clarification` として残します。

### 5. Apply the resume prompt

Evaluation outcome が `pass`、または repaired summary が safe resume に十分になったら、次を使います。

- [Resume prompt](../../prompts/resume.md)

Resume 前に、constraints、risks、verification evidence、pending next steps を再確認してください。

## Do Not

- compact-guard が `/compact` を置き換えると説明しない。
- compact-guard が Codex CLI behavior を変更すると説明しない。
- AI API keys、hosted services、package scripts、CLI automation を要求しない。
- Verification evidence がないのに tests や checks が passed したと claim しない。

## Quick Checklist

```text
Pre-compact prompt
→ Codex CLI /compact
→ Post-compact evaluation prompt
→ Repair prompt if needed
→ Resume prompt
```
