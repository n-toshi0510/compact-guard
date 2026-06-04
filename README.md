# compact-guard

compact-guard は、既存の AI coding agent compaction / compression 機能に重ねて使う、model-agnostic な policy / rubric layer です。

compact-guard は compaction engine ではありません。Codex compact、Claude Code compact、Gemini compress などの built-in compact / compress features を置き換えず、その前後で「何を保持すべきか」「compacted summary は resume してよい品質か」「修復が必要か」を判断するための Markdown documentation package を提供します。

## 提供するもの

- Markdown-based compaction policies
- Markdown-based evaluation rubrics
- pre-compact、post-compact evaluation、repair、resume 用 prompts
- bad vs good compacted-summary examples
- Codex CLI、Claude Code、Gemini CLI 向け usage docs
- policies、rubrics、prompts、examples、usage docs を改善するための contribution guidance

## 現在の MVP 状態

この実装範囲で作成済み:

- [Documentation index](docs/index.md)
- [Non-goals and portability boundaries](docs/non-goals.md)
- [Compaction policy](docs/policies/compaction-policy.md)
- [Summary evaluation rubric](docs/rubrics/summary-evaluation.md)
- [Prompt pack](prompts/)
- [Examples](examples/)
- [Usage docs](docs/usage/)
- [Contribution guide](CONTRIBUTING.md)
- [License](LICENSE)

この MVP documentation package の主要カテゴリは作成済みです。

## Core Workflow

1. compact-guard policy を使って、preservation-critical な session context を特定する。
2. 既存 AI coding agent の compact / compress feature を実行する。
3. compacted summary を compact-guard rubric で評価する。
4. 必要に応じて、resume 前に summary を repair する。
5. summary が `pass` または明示的に repaired になってから resume する。

compact-guard は compaction engine の外側に留まります。人間または既存 agent workflow が手動で適用できる、reviewable な guidance を提供します。

## Non-Goals

compact-guard は次を行いません。

- compaction 自体を実行すること
- built-in compact / compress features を置き換えること
- AI APIs を必須にすること
- hosted service を提供すること
- 1 つの vendor または 1 つの model に依存すること
- MVP で runtime code、package scripts、CLI automation、background services を追加すること

詳しくは [Non-goals](docs/non-goals.md) を参照してください。

## Documentation Map

まず [docs/index.md](docs/index.md) を読んでください。

現在利用できる主要 documents:

- [Compaction policy](docs/policies/compaction-policy.md)
- [Summary evaluation rubric](docs/rubrics/summary-evaluation.md)
- [Pre-compact prompt](prompts/pre-compact.md)
- [Post-compact evaluation prompt](prompts/post-compact-evaluation.md)
- [Repair prompt](prompts/repair.md)
- [Resume prompt](prompts/resume.md)
- [Bad summary example](examples/bad-summary.md)
- [Good summary example](examples/good-summary.md)
- [Repair walkthrough](examples/repair-walkthrough.md)
- [Codex CLI usage](docs/usage/codex-cli.md)
- [Claude Code usage](docs/usage/claude-code.md)
- [Gemini CLI usage](docs/usage/gemini-cli.md)
- [Non-goals](docs/non-goals.md)
- [Contributing](CONTRIBUTING.md)

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE).
