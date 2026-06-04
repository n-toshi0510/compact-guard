# Non-Goals and Portability

compact-guard は、既存 compact / compress workflows の周囲に置く policy / rubric layer です。compaction engine ではありません。

## Explicit Non-Goals

compact-guard は compaction 自体を実行しません。

compact-guard は Codex、Claude Code、Gemini CLI、その他 AI coding agent の built-in compact / compress features を置き換えません。

compact-guard は AI APIs を必要としません。

compact-guard は hosted service を提供しません。

compact-guard は 1 つの vendor または 1 つの model に依存しません。

compact-guard は MVP で runtime code、package scripts、CLI automation、background services、browser extensions、hosted evaluators を追加しません。

## 正しい mental model

既存 tools を compaction engine として使います。

- Codex compact
- Claude Code compact
- Gemini compress
- その他 agent-provided compact / compress features

compact-guard は、その周囲に置く guidance として使います。

- compaction 前: preservation-critical context を特定する。
- compaction 後: compacted summary を評価する。
- summary が失敗した場合: resume 前に summary を repair する。
- resume 時: 十分な context を保持した summary からのみ継続する。

## Misuse Guidance

「compact-guard を実行して session を compact する」ことを期待している場合は、代わりに利用中 agent の built-in compact / compress feature を使ってください。その後、compact-guard policy と rubric を適用し、生成された compacted summary が継続してよい品質かを判断してください。

compacted summary を compact-guard hosted evaluator に送ろうとしている場合は、送らないでください。MVP は Markdown guidance のみを提供し、hosted service はありません。

AI API key と compact-guard を統合しようとしている場合、それを MVP behavior として扱わないでください。AI API integrations は separate future spec の対象です。

## Portability Commitments

compact-guard guidance は次を満たすべきです。

- Model-agnostic
- Vendor-neutral
- Markdown として利用可能
- 人間が review 可能
- 既存 compact / compress features の周囲に適用可能

Tool-specific instructions は、特定 workflow のどこに prompt や rubric を置くかを説明してよいです。ただし core policy と rubric は、特定 model、vendor、private tool behavior に依存しないようにします。
