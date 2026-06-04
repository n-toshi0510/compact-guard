# compact-guard Documentation

この index は compact-guard の Markdown documentation package への入口です。

compact-guard は既存 compact / compress features の上に置く policy / rubric layer であり、compaction 自体は実行しません。

## 推奨 reading path

1. [Non-goals](non-goals.md) を読み、compact-guard が所有しない範囲を確認する。
2. [Compaction policy](policies/compaction-policy.md) で、compact 前後に保持すべき情報を確認する。
3. [Summary evaluation rubric](rubrics/summary-evaluation.md) で、compacted summary の resume safety を評価する。
4. Prompt pack を使い、pre-compact、post-compact evaluation、repair、resume を実行する。
5. Examples を読み、bad / good / repaired compacted summary の違いを確認する。
6. Tool-specific usage docs を読み、Codex CLI、Claude Code、Gemini CLI の workflow に適用する。

## 現在利用できる documents

- [Non-goals](non-goals.md)
- [Compaction policy](policies/compaction-policy.md)
- [Summary evaluation rubric](rubrics/summary-evaluation.md)
- [Pre-compact prompt](../prompts/pre-compact.md)
- [Post-compact evaluation prompt](../prompts/post-compact-evaluation.md)
- [Repair prompt](../prompts/repair.md)
- [Resume prompt](../prompts/resume.md)
- [Bad summary example](../examples/bad-summary.md)
- [Good summary example](../examples/good-summary.md)
- [Repair walkthrough](../examples/repair-walkthrough.md)
- [Codex CLI usage](usage/codex-cli.md)
- [Claude Code usage](usage/claude-code.md)
- [Gemini CLI usage](usage/gemini-cli.md)
- [Contribution guide](../CONTRIBUTING.md)

## MVP Documents

この MVP の主要 document categories は、policy、rubric、prompts、examples、usage docs として作成済みです。

## Terminology

- **Compaction engine**: AI coding agent が提供する既存 compact / compress feature。
- **compacted summary**: 既存 compaction engine が生成した summary。
- **safe resume**: intent、constraints、current state、evidence、risks、next actions が十分に見える resume state。
- **repair-needed**: compacted summary はまだ安全ではないが、利用可能な context から修復できる可能性が高い状態。
- **unsafe-to-resume**: critical context が欠落または歪曲されており、reconstruction なしで継続すると危険な状態。
