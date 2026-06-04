# compact-guard

Documentation-first policy and rubric layer for safer AI coding-agent compact / compress workflows.

compact-guard は、Codex compact、Claude Code compact、Gemini compress などの既存 compact / compress features の周囲に置く、model-agnostic な Markdown documentation package です。

## What is compact-guard?

compact-guard は compaction engine ではありません。Session summary を自動生成せず、built-in compact / compress features を置き換えず、AI API、hosted service、runtime code、CLI automation も必要としません。

代わりに、長時間の AI coding session を compact / compress する前後で次を判断するための policy、rubric、prompts、examples、usage docs を提供します。

- 何を必ず保持するべきか
- 何を短く要約してよいか
- 何を落としてよいか
- compacted summary は safe resume に十分か
- repair-needed または unsafe-to-resume の場合、resume 前に何を修復すべきか

## Why compact-guard?

長時間の AI coding session では、compacted summary から重要な文脈が落ちると、resume 後に古い user intent、未確認の verification claim、未解決 risks、禁止された scope に基づいて作業が進む可能性があります。

compact-guard は、既存 agent の compaction 機能をそのまま使いながら、summary の品質を reviewable にします。

- User intent、constraints、current task state、verification evidence、unresolved risks を保持する基準を明確にする。
- `pass`、`repair-needed`、`unsafe-to-resume` の outcome で resume 可否を判断する。
- Missing facts を invent せず、利用可能な context だけで repaired summary を作る。
- Codex CLI、Claude Code、Gemini CLI などの workflow に手動で差し込める。

## Quick Start

1. [Non-goals](docs/non-goals.md) を読み、compact-guard が compaction engine ではないことを確認する。
2. [Compaction policy](docs/policies/compaction-policy.md) で、compact 前に保持すべき context を確認する。
3. Compact / compress の前に [pre-compact prompt](prompts/pre-compact.md) を使う。
4. 利用中 agent の既存 compact / compress feature を実行する。
5. 生成された compacted summary を [post-compact evaluation prompt](prompts/post-compact-evaluation.md) と [summary evaluation rubric](docs/rubrics/summary-evaluation.md) で評価する。
6. 必要なら [repair prompt](prompts/repair.md) と [repair walkthrough](examples/repair-walkthrough.md) を使って resume 前に修復する。
7. `pass` または safe resume に十分な repaired summary になってから [resume prompt](prompts/resume.md) を使う。

## Basic Workflow

```text
compact-guard pre-compact prompt
→ existing agent compact / compress feature
→ compact-guard post-compact evaluation prompt
→ compact-guard repair prompt if needed
→ compact-guard resume prompt
```

compact-guard はこの workflow の外側にある documentation layer です。Compaction 自体は、利用中の AI coding agent が提供する既存機能で実行します。

## Supported Agent Workflows

compact-guard は vendor-neutral な policy / rubric を中心に設計しています。MVP では、次の手動 adoption guide を提供します。

| Agent workflow | Built-in feature | compact-guard guide |
| --- | --- | --- |
| Codex CLI | compact | [Codex CLI usage](docs/usage/codex-cli.md) |
| Claude Code | compact | [Claude Code usage](docs/usage/claude-code.md) |
| Gemini CLI | compress | [Gemini CLI usage](docs/usage/gemini-cli.md) |

これらの guides は、built-in behavior を置き換えたり変更したりするものではありません。各 tool の workflow の前後に、compact-guard の prompts と rubric をどこで適用するかを示します。

## Example: Bad Compacted Summary vs Compact-Guard Workflow

Bad compacted summary は、次のような critical context を落としがちです。

- 最新の user intent がない
- current task state がない
- verification evidence がない
- unresolved risks がない
- next actions が vague

[Bad summary example](examples/bad-summary.md) は、これらの欠落がなぜ `unsafe-to-resume` になるかを示します。

compact-guard workflow では、同じ状況を次のように扱います。

1. [Compaction policy](docs/policies/compaction-policy.md) で Must Preserve context を確認する。
2. [Summary evaluation rubric](docs/rubrics/summary-evaluation.md) で outcome を判定する。
3. `repair-needed` または `unsafe-to-resume` なら [repair prompt](prompts/repair.md) を使う。
4. Facts を invent せず、利用可能な context だけで summary を修復する。
5. [Good summary example](examples/good-summary.md) と [repair walkthrough](examples/repair-walkthrough.md) を参照して、safe resume に必要な情報が見える状態にする。

## Documentation Table

| Document | Purpose |
| --- | --- |
| [Documentation index](docs/index.md) | 全体の reading path と用語一覧 |
| [Non-goals](docs/non-goals.md) | scope boundaries と portability commitments |
| [Compaction policy](docs/policies/compaction-policy.md) | Must Preserve / May Summarize / May Drop の基準 |
| [Summary evaluation rubric](docs/rubrics/summary-evaluation.md) | `pass` / `repair-needed` / `unsafe-to-resume` の評価基準 |
| [Pre-compact prompt](prompts/pre-compact.md) | compact / compress 前に保持文脈を整理する prompt |
| [Post-compact evaluation prompt](prompts/post-compact-evaluation.md) | compacted summary を rubric で評価する prompt |
| [Repair prompt](prompts/repair.md) | 欠落や曖昧さを、invent せずに修復する prompt |
| [Resume prompt](prompts/resume.md) | safe resume 前に intent、state、evidence、risks を再確認する prompt |
| [Bad summary example](examples/bad-summary.md) | unsafe な compacted summary の例 |
| [Good summary example](examples/good-summary.md) | resume に必要な context が見える summary の例 |
| [Repair walkthrough](examples/repair-walkthrough.md) | bad summary を safer repaired summary に直す手順 |
| [Codex CLI usage](docs/usage/codex-cli.md) | Codex CLI workflow での手動適用 |
| [Claude Code usage](docs/usage/claude-code.md) | Claude Code workflow での手動適用 |
| [Gemini CLI usage](docs/usage/gemini-cli.md) | Gemini CLI workflow での手動適用 |
| [Contributing](CONTRIBUTING.md) | policy、rubric、prompts、examples、usage docs の改善方法 |

## Non-Goals

compact-guard は次を行いません。

- compaction 自体を実行すること
- built-in compact / compress features を置き換えること
- AI APIs を必須にすること
- hosted service を提供すること
- 1 つの vendor または 1 つの model に依存すること
- MVP で runtime code、package scripts、CLI automation、background services を追加すること

詳しくは [Non-goals](docs/non-goals.md) を参照してください。

## Contributing

Policy、rubric、prompts、examples、usage docs の改善は歓迎します。MVP scope と非ゴールを維持するため、変更前に [Contributing](CONTRIBUTING.md) と [Non-goals](docs/non-goals.md) を確認してください。

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE).
