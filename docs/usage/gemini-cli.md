# Gemini CLI Usage

この guide は、Gemini CLI の既存 `/compress` workflow の前後に compact-guard を手動で適用する方法を示します。

compact-guard は Gemini CLI の built-in compress behavior を置き換えず、変更せず、実行もしません。Gemini CLI が compaction engine であり、compact-guard は policy、rubric、prompts、examples を提供する surrounding layer です。

Official reference:

- Gemini CLI commands reference: https://google-gemini.github.io/gemini-cli/docs/cli/commands.html

## Assumption

この guide は、Gemini CLI session 内で `/compress` を使えることを前提にします。Gemini CLI docs は `/compress` を、entire chat context を summary に置き換え、future tasks の token use を減らしながら high level summary を保持する command として説明しています。

## Workflow Placement

### 1. Apply the pre-compact prompt

Before running `/compress`, paste or adapt:

- [Pre-compact prompt](../../prompts/pre-compact.md)

目的:

- compaction 前に preservation-critical context を整理する。
- Must Preserve、May Summarize、May Drop の分類を明確にする。
- Gemini CLI の built-in compression に依存する前に、safe resume に必要な context を見える化する。

### 2. Run the existing built-in compress feature

Gemini CLI で既存 command を実行します。

```text
/compress
```

compact-guard はこの step の内部 behavior を所有しません。

### 3. Apply the post-compact evaluation prompt

Compression 後の summary を次で評価します。

- [Post-compact evaluation prompt](../../prompts/post-compact-evaluation.md)
- [Summary evaluation rubric](../rubrics/summary-evaluation.md)

Outcome は次のいずれかです。

- `pass`
- `repair-needed`
- `unsafe-to-resume`

### 4. Apply the repair prompt if needed

Evaluation outcome が `repair-needed` または `unsafe-to-resume` の場合、resume する前に次を使います。

- [Repair prompt](../../prompts/repair.md)
- [Good summary example](../../examples/good-summary.md)
- [Repair walkthrough example](../../examples/repair-walkthrough.md)

Repair では、available recovery context にない facts を追加しないでください。Verification status が不明な場合は、passed と claim せず unknown / not recorded として残します。

### 5. Apply the resume prompt

Evaluation outcome が `pass`、または repaired summary が safe resume に十分になったら、次を使います。

- [Resume prompt](../../prompts/resume.md)

Resume prompt は、summary を correctness の証明として扱わず、action 前に intent、state、constraints、evidence、risks、next safest action を確認させます。

## Do Not

- compact-guard が `/compress` を置き換えると説明しない。
- compact-guard が Gemini CLI behavior を変更すると説明しない。
- AI API keys、hosted services、package scripts、CLI automation を要求しない。
- `/compress` の output を evaluation なしに safe resume とみなさない。

## Quick Checklist

```text
Pre-compact prompt
→ Gemini CLI /compress
→ Post-compact evaluation prompt
→ Repair prompt if needed
→ Resume prompt
```
