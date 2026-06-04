# Pre-Compact Prompt

既存 compact / compress feature を実行する前に使う prompt です。

compact-guard は compaction を実行しません。既存 AI coding agent の compact / compress feature が compaction engine です。

## Prompt

あなたは context compaction の準備をしています。

compaction を実行する前に、safe resume を守るための preservation brief を作成してください。以下の compact-guard policy categories に従ってください。

compaction 自体は実行しないでください。compact-guard が session を compact すると主張しないでください。あなたの役割は、後続の compacted summary が保持すべき内容を明確にすることです。

### Must Preserve

次の categories を capture してください。

1. **User Intent**
   - 最新の user goal
   - 現在の scope
   - 最新の explicit instruction

2. **Current Task Status**
   - 完了済み work
   - 進行中 work
   - blocked work
   - 意図的に deferred した work

3. **Constraints**
   - Project instructions
   - Non-goals
   - Approval requirements
   - Sandbox または environment limits
   - Style または language rules
   - Explicit user constraints

4. **Decisions**
   - Design decisions
   - Product または scope decisions
   - Naming decisions
   - Rejected alternatives とその理由

5. **Unresolved Questions**
   - Open questions
   - まだ validation が必要な assumptions
   - 不足している user input

6. **Risks**
   - Known risks
   - Blockers
   - Boundary concerns
   - Safety または correctness concerns

7. **Modified Files**
   - Created files
   - Edited files
   - Deleted files
   - 重要な場合は、意図的に untouched の files

8. **Commands Run**
   - Validation commands
   - Setup commands
   - Failed commands
   - Approval が必要だった commands

9. **Verification Results**
   - Passed した tests、builds、lint、review、manual checks
   - Failed した checks
   - Skipped または not run の checks

10. **Pending Next Steps**
    - 次の actionable tasks
    - Dependencies
    - Stop conditions

### May Summarize

Decision、evidence、risk、next action が明確に残る場合に限り、discussion history、repeated output、long excerpts、exploration notes を短縮してください。

### May Drop

Social filler、duplicates、decision を生まなかった dead-end exploration、diagnostic value のない verbose logs を削除してよいです。

### Output Format

次の形式で返してください。

```md
## Preservation Brief

### User Intent

### Current Task Status

### Constraints

### Decisions

### Unresolved Questions

### Risks

### Modified Files

### Commands Run

### Verification Results

### Pending Next Steps

### May Summarize

### May Drop
```

Uncertainty は見える状態で残してください。category が不明な場合は、推測せず `Unknown` または `Not established` と書いてください。
