# Contributing to compact-guard

compact-guard は documentation-first の OSS です。MVP では Markdown policies、rubrics、prompts、examples、usage docs を改善対象にします。

## Contribution Scope

歓迎する変更:

- Compaction policy の改善
- Evaluation rubric の改善
- Prompt wording の改善
- Bad / good examples の追加または改善
- Codex CLI、Claude Code、Gemini CLI usage docs の改善
- Non-goals や terminology の明確化

MVP の範囲外:

- Runtime code
- Package scripts
- AI API integrations
- Hosted services
- CLI automation
- Built-in compact / compress features の置き換え
- 特定 vendor または特定 model に依存する評価 logic

これらが必要な提案は、MVP への小変更として混ぜず、別 spec または将来フェーズとして扱ってください。

## Review Criteria

変更を提案するときは、次を確認してください。

- compact-guard が compaction engine ではなく policy / rubric layer として説明されている。
- 追加内容が model-agnostic かつ vendor-neutral である。
- `must preserve`、`may summarize`、`may drop` の用語が policy と矛盾していない。
- `pass`、`repair-needed`、`unsafe-to-resume` の outcome が rubric と矛盾していない。
- Secrets、private repository details、個人情報を examples に含めていない。
- Hosted service や AI API key を前提にしていない。

## Writing Style

- Markdown で書く。
- 読者が copy して使う prompt や checklist は、前提条件と期待結果を明確にする。
- Tool-specific guidance は usage docs に閉じ込める。
- Core policy、rubric、examples は tool-neutral を基本にする。

## Adding New Documents

新しい document を追加する場合は、README または docs index から到達できるようにしてください。未実装カテゴリにファイルを追加した場合は、該当する status 表記も更新してください。
