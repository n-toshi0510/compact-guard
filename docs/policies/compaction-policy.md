# Compaction Policy

この policy は、compacted summary が何を保持しなければならないか、何を要約してよいか、何を削除してよいかを定義します。

compact-guard は compaction を実行しません。既存 compact / compress feature を使う前後で、この policy を適用してください。

## Policy Outcomes

Session context を準備または review するときは、次の分類を使います。

### Must Preserve

safe resume に必要な情報です。この情報が欠落、曖昧、または歪曲されている場合、session を継続する前に compacted summary の repair が必要になる可能性があります。

### May Summarize

有用ではあるものの、user intent、current state、constraints、evidence、risks、next actions を変えずに短縮できる情報です。

### May Drop

resume safety、reviewability、次の implementation decision に影響しない情報です。

## Must Preserve Categories

### User Intent

ユーザーの現在の goal、明示された scope、最新 instruction を保持します。

重要な理由: compacted summary が古い goal だけを保持し、最新 direction を落とすと、resume 後の作業が drift します。

### Current Task Status

完了済み、進行中、blocked、意図的に deferred した作業を保持します。

重要な理由: resumed agent が完了済み作業を繰り返したり、blocked work を ready と誤認したりすることを防ぎます。

### Constraints

Project instructions、non-goals、approval requirements、sandbox limits、style rules、explicit user constraints を保持します。

重要な理由: Constraints は「何をしてはいけないか」を定義するため、implementation details より重要な場合があります。

### Decisions

Session 中に決まった design、product、naming、scope、trade-off decisions を保持します。

重要な理由: resumed agent が、根拠や user intent なしに確定済み decision を再度開かないようにします。

### Unresolved Questions

未回答の questions と、まだ validation が必要な assumptions を保持します。

重要な理由: 隠れた uncertainty は、resume 後に unsupported implementation choices へ変わりやすいためです。

### Risks

Known risks、rejected approaches、boundary concerns、safety issues を保持します。

重要な理由: Known hazards が context から消えると、resume が unsafe になります。

### Modified Files

現在の作業に関係する created、edited、deleted、または意図的に untouched の files を保持します。

重要な理由: File state は review、continuation、user changes の accidental overwrite 防止に必要です。

### Commands Run

重要な commands、特に validation commands、setup commands、failed commands、approval が必要だった commands を保持します。

重要な理由: resume では、どの evidence が存在し、何が failed したかを知る必要があります。

### Verification Results

Tests、build、lint、review、manual verification の results を保持します。Failures と skipped checks も含めます。

重要な理由: resumed agent が verification state を知らずに confidence を主張することを防ぎます。

### Pending Next Steps

次の actionable tasks、dependencies、stop conditions を保持します。

重要な理由: resume は partial history から intent を再構築するのではなく、concrete plan から始めるべきです。

## May Summarize

compacted summary は次を短縮してよいです。

- Preserved decision に至るまでの詳細な discussion
- 最終 result が保持されている repeated command output
- File paths、changed behavior、review-relevant details が保持されている long code excerpts
- Decision 記録後に不要になった exploration notes
- Rejection reason が保持されている alternate approaches

ただし、将来の agent がその情報を必要とする理由を消す summarization は不可です。

## May Drop

compacted summary は次を削除してよいです。

- Social filler と acknowledgements
- より明確な形で既に captured されている duplicate statements
- Decision、risk、next action を生まなかった dead-end exploration
- もはや diagnostic value がない verbose logs
- Implementation、review、resume に影響しない formatting details

情報が decision、verification result、risk、user constraint の唯一の evidence である場合、それを drop することは unsafe です。

## Review Checklist

resume 前に、compacted summary が次に答えているか確認します。

- ユーザーは今何を達成しようとしているか。
- 何が既に完了しているか。
- どの files または artifacts が変わったか。
- どの constraints と non-goals がまだ適用されるか。
- どの decisions を尊重すべきか。
- 何が unresolved または risky か。
- どの verification evidence が存在するか。
- 次に何をすべきか。

答えが欠落または materially ambiguous な場合、継続前に rubric で summary を評価してください。
